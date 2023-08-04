# sync-forest-fix
# changes

<h3>note:</h3>

* paste this file to the flocard directory to load the dependencies.
* row grouping doesn't remove the existing table heading (thead) for the corresponding table data (td).
* I used hardcoded data here in the table since I did not have access to the database.

<h3>Added row grouping to the table data</h3>

copy the below code to the head section

<head>

    <!-- Existing code here -->

    <!-- row grouping datatable -->
    <link rel="stylesheet" href="https://cdn.datatables.net/1.13.6/css/jquery.dataTables.css" />
    <script type="text/javascript" language="javascript" src="https://code.jquery.com/jquery-3.7.0.js"></script>
    <script type="text/javascript" language="javascript"
        src="https://cdn.datatables.net/1.13.6/js/jquery.dataTables.min.js"></script>
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.13.6/css/jquery.dataTables.min.css">
    <script src="https://cdn.datatables.net/1.13.6/js/jquery.dataTables.js"></script>

    <!-- Datatable css -->
    <style>
        tr.group,
        tr.group:hover {
            background-color: #dddddd4b !important;
        }
    </style>

    <!-- Existing Code here -->

</head>

here is the script 


            var groupColumn = 2;
            var table = $('#datatable').DataTable({
                columnDefs: [{ visible: false, targets: groupColumn }],
                order: [[groupColumn, 'asc']],
                displayLength: 25,
                drawCallback: function (settings) {
                    var api = this.api();
                    var rows = api.rows({ page: 'current' }).nodes();
                    var last = null;

                    api.column(groupColumn, { page: 'current' })
                        .data()
                        .each(function (group, i) {
                            if (last !== group) {
                                $(rows)
                                    .eq(i)
                                    .before(
                                        '<tr class="group"><td colspan="8">' +
                                        group +
                                        '</td></tr>'
                                    );

                                last = group;
                            }
                        });
                }
            });

            // Order by the grouping
            $('#datatable tbody').on('click', 'tr.group', function () {
                var currentOrder = table.order()[0];
                if (currentOrder[0] === groupColumn && currentOrder[1] === 'asc') {
                    table.order([groupColumn, 'desc']).draw();
                }
                else {
                    table.order([groupColumn, 'asc']).draw();
                }
            });

<h3>Dark mode fix</h3>

* dark mode fixed by changing the background colors in assets/theme.dark.min.css file. The main issue with the color value which light and not suitable with the dark theme.

  change the background-color of .table-striped (line:2157) and .table.hover (line:2161) with a dark color.

* table heading appearing white fixed by removing thead-light class (line: 676) in the html file.
