---
layout: post
title:  "ASP.Net Gridview to Excel"
date:   2013-03-17
description: It’s easy to do this. Or at least, you’d think it is. But when you allow the columns to be generated automatically, you can run into a nasty error “The controls collection cannot be modified because the control contains code blocks <% ... %>”
tags:
 - ASP.NET
 - Excel
 - C#
---
Gridviews are mostly used to give the user a good view of data in a list, like a list of orders or reservations. For advanced web-applications, these users like to export these grids of data to an Excel-file, which they can work, filter and select more easily from to use the data for calculations. This is easy to do by sending the content from the table as a ms-excel content-type in the Response of a postback:

>C#
{:.filename}
{% highlight csharp %}
protected void BtnExportToExcel(Object sender, EventArgs e)
{
     String filename = "Export" + DateTime.Now.ToString("ddMMyyyy") + ".xls";

     //Create a form to render the GridView to
     //(This will prevent an error saying the control needs to be within a form)
     HtmlForm form = new HtmlForm();

     //Clear the response and set it up to send the data as the Excel content-type
     Response.Clear();
     Response.Buffer = true;
     Response.Charset = "";
     Response.AddHeader("content-disposition", "attachment;filename=" + filename);
     Response.ContentType = "application/vnd.ms-excel";

     //Prepare the streamwriters to write to the response
     StringWriter sw = new StringWriter();
     HtmlTextWriter hw = new HtmlTextWriter(sw);

     //Populate the gridview and add it to the form
     gvData.AllowPaging = false;
     gvData.AllowSorting = false;
     gvData.EditIndex = -1;
     gvData.DataBind();
     form.Controls.Add(gvData);

     //Add the form to the page and render the contents to the streamwriter
     this.Controls.Add(form);
     form.RenderControl(hw);

     //Writer to the Response and send it out
     Response.Write(sw.ToString());
     Response.Flush();
     Response.End();
}
{% endhighlight %}

You’ll do have to change the page directive of that certain page that is returning the data, so it won’t render the “RegisterEventValidation can only be called during Render()” exception. Add ‘EnableEventValidation=”false”‘:

>ASP
{:.filename}
{% highlight csharp %}
<%@ Page Title="" Language="vb" AutoEventWireup="false"
         MasterPageFile="~/Master.Master" CodeBehind="Demo.aspx.vb"
         Inherits="Demo.Demo" EnableEventValidation="false" %>
{% endhighlight %}

This will all work without any problems most of the time. But when you allow the columns to be generated automatically or have put code blocks within the column-templates of the gridview, you’ll be presented with this nasty error: “The controls collection cannot be modified because the control contains code blocks <% … %>“.

There certainly is a way to fix this! But it involves creating a regular HTML table, and populating that table with the rows of the GridView. The GridView rows will first be checked for any other controls within them, to be sure no other code-blocks can interfere. Best is to create a function for this, so you can reuse this at any time:

>C#
{:.filename}
{% highlight csharp %}
public static Table ConvertGridToTable(GridView datagrid)
{

     Table table = new Table();

     //Must add gridlines, or an excel will be generated without any
     table.GridLines =  GridLines.Both;

     //Add the headerrow of the GridView if it's present
     if(datagrid.HeaderRow != null)
     {
          ReplaceControls(datagrid.HeaderRow);
          table.Rows.Add(datagrid.HeaderRow);
     }

     //Add the datarows of the GridView to the table
     foreach(GridViewRow row in datagrid.Rows)
     {
          ReplaceControls(row);
          table.Rows.Add(row);
     }

     //Add the footer-row if present
     if(datagrid.FooterRow != null)
     {
          ReplaceControls(datagrid.FooterRow);
          table.Rows.Add(datagrid.FooterRow);
     }

     return table;

}

private static void ReplaceControls(Control control)
{
     for(int i = 0; i < control.Controls.Count; i++)
     {
          Control current = control.Controls[i];
          if (current is LinkButton)
          {
               control.Controls.Remove(current);
               control.Controls.AddAt(i, new LiteralControl((current as LinkButton).Text));
          }
          else if (current is ImageButton)
          {
               control.Controls.Remove(current);
               control.Controls.AddAt(i, new LiteralControl((current as ImageButton).AlternateText));
          }
          else if (current is HyperLink)
          {
               control.Controls.Remove(current);
               control.Controls.AddAt(i, new LiteralControl((current as HyperLink).Text));
          }
          else if (current is DropDownList)
          {
               control.Controls.Remove(current);
               control.Controls.AddAt(i, new LiteralControl((current as DropDownList).SelectedItem.Text));
          }
          else if (current is CheckBox)
          {
               control.Controls.Remove(current);
               control.Controls.AddAt(i, new LiteralControl((current as CheckBox).Checked ? "True" : "False"));
          }

          if(current.HasControls())
          {
               ReplaceControls(current);
          }
     }
}
{% endhighlight %}

The code to export our Gridview to Excel from codebehind will then become this:

>C#
{:.filename}
{% highlight csharp %}
protected void BtnExportToExcel(Object sender, EventArgs e)
{
 
    String filename = "Export" + DateTime.Now.ToString("ddMMyyyy") + ".xls";
 
    //Clear the response and set it up to send the data as the Excel content-type
    Response.Clear();
    Response.Buffer = true;
    Response.Charset = "";
    Response.AddHeader("content-disposition", "attachment;filename=" + filename);
    Response.ContentType = "application/vnd.ms-excel";
 
    //Prepare the streamwriters to write to the response
    StringWriter sw = new StringWriter();
    HtmlTextWriter hw = new HtmlTextWriter(sw);
 
    //Populate the gridview and generate the table
    gvData.AllowPaging = false;
    gvData.AllowSorting = false;
    gvData.EditIndex = -1;
    gvData.DataBind();
    Table table = ConvertGridToTable(gvData);
 
    //Render the table to the streamwriter
    table.RenderControl(hw);
 
    //Writer to the Response and send it out
    Response.Write(sw.ToString());
    Response.Flush();
    Response.End();
 
}
{% endhighlight %}