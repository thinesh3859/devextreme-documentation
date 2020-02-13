In your application's HTML view templates, find the text/messages that you want to localize and replace it/them with keys. Write keys in the following way: *@textKey*, where *textKey* is the text to be localized that is transformed to a camel case. For instance, here is the HTML markup that contains text that will be displayed on the rendered view as is.

    <!--HTML--><div class="dx-fieldset">
        <div class="dx-field">
            <div class="dx-field-label">Bill Total:</div>
            <div id="billTotalInput" class="dx-field-value" data-bind="dxNumberBox: {
                value: billTotal,
                placeholder: 'Type here...',
                valueChangeEvent: 'keyup',
                min: 0
            }"></div>
        </div>
    </div>

And here is the HTML markup where the text to be displayed on the view is replaced with keys.

    <!--HTML--><div class="dx-fieldset">
        <div class="dx-field">
            <div class="dx-field-label">@billTotal:</div>
            <div id="billTotalInput" class="dx-field-value" data-bind="dxNumberBox: {
                value: billTotal,
                placeholder: '@typeHere...',
                valueChangeEvent: 'keyup',
                min: 0
            }"></div>
        </div>
    </div>

[note] The @-keys should be specified in the HTML markup only.