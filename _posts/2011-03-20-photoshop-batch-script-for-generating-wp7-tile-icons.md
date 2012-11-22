--- 
layout: page
title: Photoshop batch script for generating WP7 tile icons
funnelweb_id: 75
date: 2011-03-20 13:00:00 +11:00
tags: "photoshop wp7 "
comments: true
---

There are a few different ways you can do "live tiles" in WP7, but the majority of them involve generating images that differ only in the number thats included. ie, "youricon" + "unread/missed messages".

The problem with this is you need to either generate the icons server side and then having to download them to the phone, or generating them with your app/xap.

Or you can run this script, and it'll generate *N* icons by finding a text layer named "NumberLayer", editing the number, and saving. Change the number, it generates more. Why go with an existing layer rather than inserting it? That way you can get all the lining up done via a WYSIWYG tool rather than guessing in code.

Save the file as a JSX, use it in photoshop by going File -> Scripts -> Browse (to the file), and it'll work on the currently open/active file.


    //get the active document
    var doc = app.activeDocument;
    var docName = app.activeDocument.name.match( /(.*)(\.[^\.]+)/ )[1];
    var filepath = app.activeDocument.path;
    var l = doc.layers.getByName("NumberLayer");
    
    displayDialogs = DialogModes.NO;
    saveOptions = new PNGSaveOptions();
    
    if(l.kind == LayerKind.TEXT) {
        var ti = l.textItem;
        for(var i=1; i<=20;i++)
        {
            ti.contents = i;
            var folder = new Folder(filepath);
            var newFile = new File(folder + "/" + docName + i + ".png");
            doc.saveAs(newFile, saveOptions);
        }
    }

**Example**  
![example][2]


  [2]: /images/postimages//TileIcon1.png
