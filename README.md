A simple browser-based application of SheetJS and Vue to display Excel data in an HTML template.

Being browser-based, the user has to specify the input Excel file. 

To use in an HTML file with the Vue templating, include 
the necessary JavaScript (SheetJS, Vue, and xls-vue), and call **initXlsVue()** with
the Vue app object and the ID of input file element, e.g.,

```
<!DOCTYPE html>
<html>
  ...
  <body>
    <input type="file" id="file-source">
    <div class="container" id="xls-view">
      ...
    </div>
    
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.11.17/xlsx.full.min.js"></script>
    <script src="./xls-vue.js"></script>
    <script type="text/javascript">
      const app = new Vue({ el: '#xls-view', data: { ... } });
      initXlsVue(app, 'file-source');
    </script>
  </body>
</html>
```

The spreadsheet should contain one or more worksheets, each with column headers. 

The app will create
a JavaScript object with a key for every sheet name. The value for each key will be the 
JSON output returned by the SheetJS function 
[XLSX.utils.sheet_to_json()](https://docs.sheetjs.com/#json) applied to each sheet.

For example, with the above HTML, selecting the sample file **speakers.xlsx** will
set the Vue data field to 

```
{
  "Speakers": [
    { "Name": ..., "Image": ..., "Url": ..., "Title": ... },
    { "Name": ..., "Image": ..., "Url": ..., "Title": ... },
    ...
  ]
}
```

An optional third argument can be passed to **initXlsVue()** to process the worksheet data
before storing in the Vue object. The argument should be a function of a list of items and 
the name of the worksheet. It should return a list of items.