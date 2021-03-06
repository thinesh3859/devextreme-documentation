To export or print the **Funnel**, a user clicks the *"Exporting/Printing"* button and selects a command from the drop-down menu. The **Print** command opens the browser's **Print** window that lets the user select preferred printing settings and send the print job to the printer. The other commands save a file of the selected format in the user's local storage.

![Funnel Export Menu](/images/Funnel/visual_elements/export-menu.png)

You can enable both exporting and printing by setting the [export](/api-reference/20%20Data%20Visualization%20Widgets/BaseWidget/1%20Configuration/export '/Documentation/ApiReference/Data_Visualization_Widgets/dxFunnel/Configuration/export/').**enabled** option to **true**. If you need only exporting to be available to the user, disable printing by assigning **false** to the **export**.[printingEnabled](/api-reference/20%20Data%20Visualization%20Widgets/BaseWidget/1%20Configuration/export/printingEnabled.md '/Documentation/ApiReference/Data_Visualization_Widgets/dxFunnel/Configuration/export/#printingEnabled') option.

---
##### jQuery

    <!--JavaScript-->$(function() {
        $("#funnelContainer").dxFunnel({
            // ...
            export: {
                enabled: true,
                printingEnabled: false
            }
        });
    });

##### Angular

    <!--HTML--><dx-funnel ... >
        <dxo-export
            [enabled]="true"
            [printingEnabled]="false">
        </dxo-export>
    </dx-funnel>

    <!--TypeScript-->
    import { DxFunnelModule } from "devextreme-angular";
    // ...
    export class AppComponent {
        // ...
    }
    @NgModule({
        imports: [
            // ...
            DxFunnelModule
        ],
        // ...
    })

---

If you want to restrict the set of formats available for exporting, change the **export**.[formats](/api-reference/20%20Data%20Visualization%20Widgets/BaseWidget/1%20Configuration/export/formats.md '/Documentation/ApiReference/Data_Visualization_Widgets/dxFunnel/Configuration/export/#formats') array. You can also specify the default name for the exported file using the [fileName](/api-reference/20%20Data%20Visualization%20Widgets/BaseWidget/1%20Configuration/export/fileName.md '/Documentation/ApiReference/Data_Visualization_Widgets/dxFunnel/Configuration/export/#fileName') option.

---
##### jQuery

    <!--JavaScript-->$(function() {
        $("#funnelContainer").dxFunnel({
            // ...
            export: {
                enabled: true,
                formats: ["PNG", "JPEG"],
                fileName: "exported_funnel"
            }
        });
    });

##### Angular

    <!--HTML--><dx-funnel ... >
        <dxo-export
            [enabled]="true"
            [formats]="['PNG', 'JPEG']"
            fileName="exported_funnel">
        </dxo-export>
    </dx-funnel>

    <!--TypeScript-->
    import { DxFunnelModule } from "devextreme-angular";
    // ...
    export class AppComponent {
        // ...
    }
    @NgModule({
        imports: [
            // ...
            DxFunnelModule
        ],
        // ...
    })

---
