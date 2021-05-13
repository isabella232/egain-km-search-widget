# eGain-KM-Widgets-Search-Widget

# About
The Search Widget renders an unordered list in an HTML page based on an initialization search string parameter. The list is a result of articles which matches the search parameter from eGain knowledge base. The article results list can also be updated dynamically by calling a javascript function with a corresponding search parameter.


![search-widget](https://user-images.githubusercontent.com/83938216/117610210-f804ec80-b17e-11eb-92d7-ff9aa85213f9.png)


# Prerequisites
1. eGain system url with context root.
2. eGain knowledge base portal id 
3. Enable CORS in the eGain application. This is required in order to make the web service APIs work without CORS issues. Follow section "Enabling Cross-Origin Resource Sharing" of the product guide "eGain Administrator’s Guide to Administration Console".

# Using The Search Widget
This sample application uses search widget to fetch the corresponding articles from eGain knowledge using the short description. Initially search will be done using pre populated short description. Search results will be updated dynamically as we change the short description.

1. Clone or download the Sample Application from git hub repository https://github.com/snehaldatar/eGain-KM-Search-Widget.
1. Change below attributes as required in the index html file.
    1. your_egain_system_url_with-context_root
    1. your_kb_portal_id
1. Now you should be able to fetch the search results from your eGain knowledge portal by loading the index html in browser.


# Configurations
This widget can be configured in the customer's HTML page which can be rendered as desired (dynamically or page load). There is no restriction on combination of widgets i.e., the widgets can be combined with each other in any order in the page.

Each widget follows a pattern in which the attributes of the placeholder HTML element can be configured.

## Widget Library
The library provides a global object "eGainUI" to invoke the functions. It also loads a name-spaced version of JQuery. The name of the JQuery object is same "eGainUI" such that it will not conflict with any other JQuery library customers may have.

The library must be included in the page(s) that are going to enable eGainUI Widgets. The recommendation is to deploy this reference in a global header or footer so the widgets can be deployed dynamically to any page without further site modifications.

Recommendation:
* Please use it in the header: If the JavaScript library is being used by making a javascript function call.
* Please use it in the footer: If the page/document load event is used to trigger the widget.

Script tag to be added to load the eGain JavaScript library is -

```JavaScript
<script type="text/javascript" src="//your_egain_system_url_with-context_root/widgets/libs/egain-ui-2.0.0.min.js"></script>
```


## Adding Widget to the Page
Once the JavaScript reference to the library is complete, the placeholder HTML elements for widget can simply be added to the page. 

For e.g., the following HTML placeholder DIV will render an unordered list of knowledge articles for the corresponding portal with the search term of "test"


```html
<div id="HTML-element-Id"
    data-egain-role="search-widget"
    data-egain-search-param="test"
    data-egain-portal-id="portal_id"
    data-egain-host="//your-eGain-domain.com/system_context_root/"
    data-egain-template-name="silver"
    data-egain-locale="en-US">
</div>
```

The API to call it dynamically is as follows:
```
<script>
    $(document).ready(function () {
        eGainUI('#HTML-element-Id').SearchWidget("search-term", function () {
            //Code to be executed once the widget is successfully loaded
        });
    });
</script>
```

## Placeholder HTML Element Attributes

|Attribute Name | Description | Type | Value | Default | Required |
|---------------|-------------|------|-------|---------|----------|
|data-egain-host |eGain server name. It is being used to call the eGain web-service APIs on this server.|	String	| E.g., //widget.egain.com/system/ |	NONE |	Yes |
|data-egain-template-name|Template of the widget for display. It may be used to control the look and feel of the widget.|	String|	E.g., kiwi |	kiwi |	No |
|data-egain-link-target | The target window to be used to open the link. |	String	| Should be same as target attribute of the HTML Anchor element. For e.g. _blank or _self etc. |	As default set by browser. |	No
|data-egain-locale | The locale of the knowledge data.|	String| Format "languagecode-CountryCode". For e.g., "en-US". Should match with the MLKB language. | en-US|	No|
|data-egain-portal-id | Id of the Portal created through eGain KB console (confirm this with eGain deployment team). | Numeric	| E.g., 201700000001000	| NONE	| Yes|
|data-egain-search-param | The term that should be searched in the articles corresponding to the portal Id provided. | String | Eg. Sample | None | Yes


## Styling the output
The library will automatically add eGain-specific CSS classes to each of the elements rendered. It also allows the customer to control the look & feel of the widget through customer specific CSS.

```css
/*
 * This CSS class is added to H1 element rendered
 */
.eGain-Search-Results-h1 {
    /* Add your styles here */
}
  
/*
 * This CSS class is added to each UL element rendered
 */
.eGain-Search-Results-Ul {
    /* Add your styles here */
}
  
/*
 * This CSS class is added to each LI element rendered in the list
 */
.eGain-Search-Results-Li {
    /* Add your styles here */
}
  
/*
 * This CSS Class is added to each anchor tag
 */
.eGain-Search-Results-A {
    /* Add your styles here */
}
```

By providing the definitions to the above CSS classes that is referenced from the page where the widget is being rendered, the non-formatted or non-styled output can be formatted or styled to suit the customer's branding easily. 

## L10N and Callback support:
By default the widget will be loaded with en-US locale and same will be passed to the WS-APIs. In order to fetch the MLKB content update the "data-egain-locale" attribute to fetch the content in desired locale. However the labels and headings will still be in English language. Hence in order to change the labels to the desired language and text callbacks must be used. Callbacks can be registered both for static and dynamically widget.

# Callback in static widget: 
The callback handler must be bound at the end of the page (where static widget is embedded) in a script section. Following is the sample code for registering callback and updating the heading in the callback:

### Callback for static widgets
```
eGainUI('#HTML-element-Id').on("eGainUI.callback", function () {
    eGainUI('#HTML-element-Id h1').html("Search Widget - Static");
});
```
### Callback in dynamic widgets: 
The callback handler must be registered when triggering the dynamic widget. Following is the sample code for registering callback and updating the heading in the callback:
```
eGainUI('#HTML-element-Id').SearchWidget("account", function () {
    eGainUI('#HTML-element-Id h1').html("Search Widget - Dynamic");
});
```
