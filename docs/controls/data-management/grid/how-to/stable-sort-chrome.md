---
title: Implement Stable Sort in Chrome
page_title: Implement Stable Sort in Chrome | Kendo UI Grid
description: "Learn how to implement a stable sort in Google Chrome when using the Kendo UI Grid widget."
slug: howto_implement_stable_sortin_chrome_grid
---

# Implement Stable Sort in Chrome

The implementation of the built-in sorting algorithm in Google Chrome [is not guaranteed to be stable](https://code.google.com/p/v8/issues/detail?id=90). A [non-stable sorting algorithm](https://en.wikipedia.org/wiki/Sorting_algorithm#Stability) may cause items with the same sorting order to change places.

The example below demonstrates that when run in Chrome and items are sorted by **Address**, they change places. This is visible if you look at the **Name** column. The example represents a typical case where a non-stable sort causes side effects.

###### Example

```html
    <div id="grid"></div>
    <script>
      var dataSource = new kendo.data.DataSource({
        data: [
          { Name: "Jane Doe",    Address:"" },
          { Name: "Jane Doe 1",  Address:"" },
          { Name: "Jane Doe 2",  Address:"" },
          { Name: "Jane Doe 3",  Address:"" },
          { Name: "Jane Doe 4",  Address:"" },
          { Name: "Jane Doe 5",  Address:"" },
          { Name: "Jane Doe 6",  Address:"" },
          { Name: "Jane Doe 7",  Address:"" },
          { Name: "Jane Doe 8",  Address:"" },
          { Name: "Jane Doe 9",  Address:"" },
          { Name: "Jane Doe 10", Address:"" }
        ]
      });

      $("#grid").kendoGrid({
        dataSource: dataSource,
        sortable:true,
        columns: [{
          field: "Name"
        }, {
          field: "Address"
        }]
      });
    </script>
```

The solution for this issue is to add a position field and use it to maintain stability. This is done through a custom [`columns.sortable.compare function`](/api/javascript/ui/grid#configuration-columns.sortable.compare).

###### Example - a stable sort function using a position field

```html
    <div id="grid"></div>
    <script>
      var dataSource = new kendo.data.DataSource({
        data: [
          { _position: 1,  Name: "Jane Doe",    Address:"" },
          { _position: 2,  Name: "Jane Doe 1",  Address:"" },
          { _position: 3,  Name: "Jane Doe 2",  Address:"" },
          { _position: 4,  Name: "Jane Doe 3",  Address:"" },
          { _position: 5,  Name: "Jane Doe 4",  Address:"" },
          { _position: 6,  Name: "Jane Doe 5",  Address:"" },
          { _position: 7,  Name: "Jane Doe 6",  Address:"" },
          { _position: 8,  Name: "Jane Doe 7",  Address:"" },
          { _position: 9,  Name: "Jane Doe 8",  Address:"" },
          { _position: 10, Name: "Jane Doe 9",  Address:"" },
          { _position: 11, Name: "Jane Doe 10", Address:"" }
        ]
      });

      $("#grid").kendoGrid({
        dataSource: dataSource,
        sortable:true,
        columns: [{
          field: "Name"
        }, {
          field: "Address",
          sortable: {
            compare: function(a, b, descending) {
              if(a.Address !== b.Address)
              {
                return a.Address - b.Address;
              }

              if (descending) {
                return b._position - a._position;
              } else {
                return a._position - b._position;
              }
            }
          }
        }]
      });
    </script>
```

## See Also

Other articles on Kendo UI Grid and how-to examples:

* [JavaScript API Reference](/api/javascript/ui/grid)
* [How to Add Cascading DropDownList Editors]({% slug howto_add_cascading_dropdown_list_editors_grid %})
* [How to Add Tooltip to Grid Cells]({% slug howto_add_tooltipto_grid_cell_record_grid %})
* [How to Copy Data from Excel]({% slug howto_copy_datafrom_excel_grid %})
* [How to Create Checkbox Filter Menu]({% slug howto_create_checkbox_filter_menu_grid %})
* [How to Customize Rows and Cells Based on Data Item Values]({% slug howto_customize_rowsand_cells_basedon_dataitem_values_grid %})
* [How to Drag and Drop Rows between Grids]({% slug howto_dragand_drop_rows_between_twogrids_grid %})
* [How to Enable ForeignKey Column Sorting by Text]({% slug howto_enable_foreignkey_sotringby_text_grid %})
* [How to Filter Array Columns Using MultiSelect]({% slug howto_filetr_array_columns_using_multiselect_grid %})
* [How to Filter Grid as You Type]({% slug howto_filter_gridas_you_type_grid %})
* [How to Initialize Data Attribute with Detail Template]({% slug howto_initialize_data_attributewith_detail_template_grid %})
* [How to Load and Append More Records While Scrolling Down]({% slug howto_loadand_append_morerecords_while_scrollingdown_grid %})
* [How to Perform CRUD Operations with Local Storage Data]({% slug howto_perform_crud_operationswith_local_storage_data_grid %})
* [How to Persist Collapsed State of Grouped Records]({% slug howto_persist_collapsed_stateof_grouped_records_grid %})
* [How to Persist Expanded Rows after Refresh]({% slug howto_persist_expanded_rows_afetrrefresh_grid %})
* [How to Preserve Grid State in a Cookie]({% slug howto_preserve_gridstate_inacookie_grid %})
* [How to Set Cell Color Based on ForeignKey Values]({% slug howto_set_cell_color_basedon_foreignkey_values_grid %})
* [How to Show Tooltip for Column Records]({% slug howto_show_tooltipfor_column_records_grid %})
* [How to Sort Multiple Checkbox Filter]({% slug howto_sort_multiple_checkbox_filter_grid %})
* [How to Update Toolbar Content Using MVVM Binding]({% slug howto_update_toolbar_content_using_mvvmbinding_grid %})
* [How to Use Checkboxes inside Column Menus]({% slug howto_use_checkboxes_inside_column_menu_grid %})
* [How to Use Draggable Elements with Multiselection Enabled]({% slug howto_use_draggable_elements_multiselection_enabled_grid %})
* [How to Use Grid in Kendo UI SPA Application]({% slug howto_use_gridin_kendouispa_app_grid %})
* [How to Use MultiSelect for Column Filtering]({% slug howto_use_multiselect_forcolumn_filtering_grid %})
* [How to Use Nested Chart]({% slug howto_use_nested_charts_grid %})
* [How to Use Nested Model Properties]({% slug howto_use_nested_model_properties_grid %})
* [How to Use WebAPI with Server-Side Operations]({% slug howto_use_webapi_withserverside_operations_grid %})
