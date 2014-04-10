
# Adding Help Text Tutorial

### Overview
Broadleaf Commerce can be configured to show help text, tooltips, and hints for any field managed in the admin.  

The following image shows the different types of help that are supported by default.

![Field Help Examples](/help-examples.png)

**Help Text** 
Help Text will appear just below the input field.    In the image above, see the phrase, "This is the help text." 

**Hint**
Hints appear in a field when it is empty.    The text "This is a hint." in the above example.

**Tooltip**
Tooltips will surface a help icon next to the field label.   When the user hovers over the help icon, the text will display.   See above with the "This is a tooltip".


### Adding Help Through Annotations 
When you need to add help to a custom entity or extension in your codebase, the easiest way to do that is using admin annotations.  

For example, see the following code shows a field that has been annotated to produce the result in the image above.

```java
    @Column(name = "NAME", nullable=false)
    @AdminPresentation(group = "Info"            
            helpText = "This is help text",
            hint = "This is a hint.",
            tooltip = "This is a tooltip.")
    protected String name;
```


### Adding Help to Broadleaf Out of Box Fields 
[[Admin Metadata Overrides]] can be used to add or modify help to the out of box Broadleaf domain.

For example, to add tooltip help to the out of box Category domain, the following would work. 

```java
Example

```


### Internationalization Considerations 
In the examples above, the text was provided directly in the annotation or override.    Broadleaf will first attempt to resolve the text by looking in _Message Property_ files.

So, if the code above was changed to use the following ... 

```java
    @Column(name = "NAME", nullable=false)
    @AdminPresentation(group = "Info"            
            helpText = "myfield_helptext",
            hint = "myfield_hint",
            tooltip = "myfield_tooltip")
    protected String name;
```

Then in your message files _(admin-messages.properties)_ in the sample Broadleaf Demo Site.  

You would simply add lines for each of the values above

```
myfield_helptext=This is the help text.
myfield_hint=This is the hint.
myfield_tooltip=This is the tooltip.
```
 
To internationalize the help, you would add to the Java standard i18n compatible property file extension.   For example, _admin-messsages_fr.properties_ for french.    
