---
layout: page
title: Form Saver
permalink: /form-saver/
---

<p>make all classes and js vars consistent</p>
<p>on upload check if field names are the same, if not show error</p>


<form id="aform" name="myform">
    <input type="text" name="field1" placeholder="enter something damnit">
    <input type="text" name="fieldnew" placeholder="uggh">
    <select name="selectyo">
        <option>select a val</option>
        <option value="selectval">select this bitch</option>
    </select>
    <input type="text" name="somethingnew" placeholder="wuut">
</form>

<style>
.form-saver {margin:20px 0;}
</style>

<div class="form-saver">
    <div class="save"><a href="" id="saveForm" >save form fields</a></div>
</div>

<input type="file" id="file-input" />
<div id="fileOutput"></div>

<script>
//saving file to disk
$(".save").click(function() {
    var formObject = {};
    $('#aform *').filter(':input').each(function(){
        var fieldname = $(this).attr('name'), fieldvalue = $(this).val(), tempObject = {};
        tempObject[fieldname] = fieldvalue;
        $.extend(formObject, tempObject);
    });
    download(JSON.stringify(formObject));
});

//creates the download file and adds it to the href
function download(text) {
  var link = document.getElementById("saveForm");
  var file = new Blob([text], {type: "text/plain"});
  link.href = URL.createObjectURL(file);
  currentdate = new Date($.now());
  filename = "mysite_" + (currentdate.getMonth()+1) + currentdate.getFullYear() + currentdate.getMinutes() + currentdate.getSeconds();
  link.download = filename;
}

//reading file from disk
function readSingleFile(e) {
  var file = e.target.files[0];
  if (!file) {
    return;
  }
  var reader = new FileReader();
  reader.onload = function(e) {
    var contents = e.target.result;
    populateForm(JSON.parse(contents));
  };
  reader.readAsText(file);
}

//loop thru json file and populate all associated fields
function populateForm(contents) {
    $.each(contents, function(i, val) {
        field = $('[name='+i+']');
        if ( field.length ) {
            if (field.val().length) {console.log(field.val().length+i+' is already populated, overwrite?');}
            field.val(val);
        } 
        else {
           $('#fileOutput').append('<div class="fieldError">'+i+' not found in form</div>'); 
        }
    });
}

document.getElementById('file-input').addEventListener('change', readSingleFile, false);
</script>
