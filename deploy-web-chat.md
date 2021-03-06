---

copyright:
  years: 2015, 2020
lastupdated: "2020-02-14"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:preview: .preview}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Integrating with your own website ![Beta](images/beta.png)
{: #deploy-web-chat}

Embed your assistant in your company website as a chat widget.
{: shortdesc}

When you create a Web Chat integration, code is generated that calls a script written in JavaScript. The script instantiates a unique instance of your assistant. You can then copy and paste the HTML `<script>` element into any page or pages on your website where you want users to be able to ask your assistant for help.

![Plus or Premium plan only](images/plus.png) This feature is available to Plus or Premium plan users only.

If you are participating in the **Early Access Program**, follow the instructions in this topic instead: [Beta: Integrating with your own website](/docs/assistant?topic=assistant-beta-deploy-web-chat).
{: tip}

## Create a Web Chat instance to add to your website
{: #deploy-web-chat-task}

To add the assistant to a web page on your company website, complete the following steps:

1.  From the Assistants page, click to open the assistant tile that you want to deply to your site.

1.  From the *Integrations* section, click the **Web Chat** tile.

1.  **Optional**: Change the Web Chat integration name from *Web Chat* to something more descriptive.

1.  Click **Create** to generate the script.

    A code snippet is created and added to the bottom of the page that contains an HTML `<script>` element. The script tag calls JavaScript code that is hosted on an IBM site. The code instantiates an instance of your assistant within a Web Chat widget. The generated code includes a region and unique integration ID. Do not change these parameter values.

1.  **Optional**: Customize the chat. You can make the following changes:

    - **Public assistant name**. Name by which the assistant is known to users. This name is displayed in the header of the chat window. 
    - **Font family**. List one or more font styles that you want applied to the text that is displayed in the chat window. Separate multiple font names with commas. The first font family in the list is used unless the web page does not support it, in which case the next font family in the list is used. Surround font family names that include spaces with single quotation marks. For example, `Georgia,'Helvetica Neue'`. 
    
      If you don't specify a font, these fonts are used: `IBMPlexSans, Arial, Helvetica, sans-serif`. 

    - **Primary color**. Sets the color of the Web Chat header.

      Click the white dot to open a color switcher where you can choose a color. The color is saved as an HTML color code, such as `#FF33FC` for pink and `#329A1D` for green. Alternatively, you can add an HTML color code directly to the field to set the color.

    - **Secondary color**: Sets the color of the user input message bubble.

    - **Accent color**. Sets the color of interactive elements, including:

      - Chat launcher button that is embedded in your web page
      - Send button associated with the input text field
      - Input text field border when in focus
      - Marker that shows the start of the assistant’s response
      - Border of a button after it is clicked
      - Typing indicator that is shown to repesent a pause response
      - Active state for dropdown color of border

    Style changes you make are immediately applied to the preview that is shown on the page, so you can see how your choices impact the style of the chat UI.

1.  Copy the `<script>` HTML element.

    Do not click *Save changes* until you are done with making Web Chat edits; it closes the page.
    {: tip}

1.  Open the HTML source for a web page on your website where you want the chat window to be displayed. Paste the code snippet into the page.

    Paste the code as close to the closing `</body>` tag as possible to ensure that your page renders faster.
    {: tip}

    The following HTML snippet is the source for a test page that you can copy and save as a file with a .html extension for testing purposes. You would replace the script element block here with the script elements you copied from the Web Chat integration setup page.

    ```html
    <html>
    <head></head>
    <body>
        <title>My Test Page</title>
        <p>The body of my page.</p>
        <!-- copied script elements -->
        </body>
    </html>
    ```
    {: codeblock}

1.  Refresh the web page.

    ![Chat icon](images/web-chat-icon.png) 
    
    The Web Chat launcher icon is displayed at the end of the page. The icon is blue unless you customize the accent color.

    The placement of the Web Chat icon is always the same regardless of where you paste the script element into the web page source.
    {: important}

    For information about the web browsers that are supported by the Web Chat, see [Browser Support](https://watson-developer-cloud.github.io/assistant-web-chat/documentation.html#browser-support){: external}.

1.  Click the icon to open the chat window and talk to your assistant.

    ![Web chat window](images/web-chat-window.png)

1.  Paste the code snippet into each web page where you want the assistant to be available to your customers.

    You can paste the same script tag into as many pages on your website as you want. Add it anywhere where you want users to be able to reach your assistant for help. However, be sure to add it only one time per page.
    {: tip}

1.  Submit test utterances from the chat widget that is displayed on your web page to see how the assistant responds.

    No responses are returned until after you create a dialog skill and add it to the assistant.
    {: note}

    If you don't extend the session timeout setting for the assistant, the dialog flow for the current session is restarted after 60 minutes of inactivity. This means that if a user stops interacting with the assistant, after 60 minutes, any context variable values that were set during the previous conversation are set to null or back to their initial values.

1.  Click **Save changes** to save the Web Chat name and any customization information that you added and close the integration page. Alternatively, you can click the **X** to close the page. 

    The Web Chat instance is created as soon as you click the *Create* button, and does not need to be saved.

## Extending the Web Chat
{: #deploy-web-chat-extend}

You can make more advanced customizations and extend the capability of the Web Chat by using the {{site.data.keyword.conversationshort}} Web Chat toolkit on [GitHub](https://watson-developer-cloud.github.io/assistant-web-chat/index.html){: external}.

If you choose to use the provided methods, you implement them by editing the code snippet that was generated earlier. You then embed the edited code snippet into your web page.

### Setting and passing context variable values
{: #deploy-web-chat-set-context}

For example, the following edited script preserves the context for the conversation. In addition, it adds an `$ismember` context variable and sets it to `true`.

```html
<script src="https://assistant-web.watsonplatform.net/loadWatsonAssistantChat.js"></script>
<script>
  // Following the v2 message API, we add some items to context.
  function preSendhandler(event) {
    event.data.context = event.data.context || {};
    event.data.context.skills = event.data.context.skills || {};
    event.data.context.skills['main skill'] = event.data.context.skills.main_skill || {};
    event.data.context.skills['main skill'].user_defined = event.data.context.skills['main skill'].user_defined || {};
    event.data.context.skills['main skill'].user_defined.ismember = true;
  }
</script>
<script>
  window.loadWatsonAssistantChat({
    integrationID: '{INTEGRATION ID}',
    region: '{REGION}'
  }).then(function(instance){
    // When this promise returns, we know WatsonAssistantChat is ready.
    instance.on({ type: "pre:send", handler: preSendhandler });
    instance.render();
  });
</script>
```
{: codeblock}

You can reference the `$ismember` context variable from your dialog. For example, the following screen shot shows a dialog node that conditions on #General_Greetings. It has multiple conditioned responses. The first response checks whether the current user is a member of your Rewards Program by checking for the presence of the `$ismember` context variable. If the variable is present, the response addresses the user as a member. The next response has a more generic greeting.

![Shows multiple conditioned responses in a dialog node, one of which references the ismember context variable](images/web-chat-use-context-var.png)

### Adding user identity information
{: deploy-web-chat-userid}

If you want to perform tasks that require you to know the user who submitted the user input, then you must pass the user ID to the Web Chat integration. Such tasks include the following:

- User-based service plans use the `user_id` associated with user input for billing purposes. See [User-based plans](/docs/assistant?topic=assistant-services-information#services-information-user-based-plans). 
- The ability to delete any data created by someone who requests to be forgotten requires that a `customer_id` be associated with the user input. When a `user_id` is defined, the product can reuse it to pass a `customer_id` parameter. See [Labeling and deleting data](/docs/assistant?topic=assistant-information-security#information-security-gdpr-wa).

To support these user-based capabilities, you must add the `options.userID` method to the codeblock before you paste it into your web page.

In the following example, the user ID `L44556677` is added to the script codeblock.

```html
<script src="https://assistant-web.watsonplatform.net/loadWatsonAssistantChat.js"></script>
<script>
  window.loadWatsonAssistantChat({
    integrationID: '{INTEGRATION ID}',
    region: '{REGION}',
    userID: 'L44556677'
  }).then(function(instance){
    // When this promise returns, we know WatsonAssistantChat is ready.
    instance.on({ type: "pre:send", handler: preSendhandler });
    instance.render();
  });
</script>
```
{: codeblock}
