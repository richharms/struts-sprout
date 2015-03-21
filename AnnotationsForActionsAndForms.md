# Introduction #

In the process of looking for a way to move a large project (1523 Actions, 612 ActionForms) built using Struts, and configured using XDoclet 1.2 to "something else more robust," I found Sprout. Given its simplicity it seems like a natural base to use in trying to eliminate the rather fragile, and no longer well maintained XDoclet 1.2.

Originally, I intended to move the project over to using annotations as new code was added, but unfortunately XDoclet (actually, xjavadoc-1.5-snapshot050611) experienced a problem with the following code in the annotation classes:

```
   SproutProperty[] properties() default { };
```

It really didn't like the "default { }" part of that. I was able to convert the project over to using only annotations and eliminate XDoclet completely with a little over one day's worth of effort. BBEdit and some regular expression find/replaces transformed the code in virtually no time at all.

# Action Example #

Previously:

```
/**
 * @struts.action
 *     path="/Item/ItemCategory/Add"
 *     className="com.echothree.view.client.web.struts.sslext.config.SecureActionMapping"
 *     scope="request"
 *     name="ItemCategoryAdd"
 *     validate="false"
 *
 * @struts.action-set-property
 *     property="secure"
 *     value="false"
 *
 * @struts.action-forward
 *     name="Display"
 *     path="/action/Item/ItemCategory/Main"
 *     redirect="true"
 *
 * @struts.action-forward
 *     name="Form"
 *     path="/item/itemcategory/add.jsp"
 */
```

Now:

```
@SproutAction(
    path = "/Item/ItemCategory/Add",
    mappingClass = SecureActionMapping.class,
    name = "ItemCategoryAdd",
    properties = {
        @SproutProperty(property = "secure", value = "false")
    },
    forwards = {
        @SproutForward(name = "Display", path = "/action/Item/ItemCategory/Main", redirect = true),
        @SproutForward(name = "Form", path = "/item/itemcategory/add.jsp")
    }
)
```


# ActionForm Example #

Previously:

```
/**
 * @struts.form
 *     name="ItemCategoryAdd"
 */
```

Now:

```
@SproutForm(name="ItemCategoryAdd")
```