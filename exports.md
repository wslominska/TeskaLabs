# LogMan.io Export beta

Create, run, remove or download exports.
The new Export funtionality is now in *beta*.

## List of Exports
Green exports are successfully completed exports. Yellow exports are in progress. Failed exports are in red and scheduled exports are shown in blue. Click on the name of each export to open its detailed view. You can download or delete each export. The list of exports is sorted from the most recent expor to the most old. You can search the list by name using the search bar. You can make new export.

## Export Detail
You can see complete information about a specific export on the Detail screen. Here you can also download and delete. The "Restart" runs the export again as is or with your custom changes. Deleting a scheduled export does not delete the exports that were generated from it.

## Start Export
The Start Export screen offers you pre-made Exports from the Library. You can run these directly or modify them and run them with your custom changes. The "Custom" button takes you to a blank form where you can create a self-made export.

## Custom Export
First, select the Data Source.
This selection will determine the other export options.

Select the output.
If you select CSV output, you must enter the column names.
"Add new header" button adds more columns. You can change the order of the columns by dragging.
The same order of columns will be in the resulting table.
If you do not fill in the columns or their names do not match the search terms in the database, your table will be empty.

Target option depends on the configuration of the Export function and can incude "e-mail" and "jupyter" targets.
Jupyter exports the files directly to jupyter notebook integrated within LogMan.io.
Choose a predefined e-mail template from the Library or create there your own from there.

"Add schedule" button allows you to plan your export.
Fill in date and time for one-off exports. You must stritly follow YYYY-MM-DD HH:mm format. Your date might look like this: *2023-01-01 12:00*.
The second variant handes periodicaly scheduled exports. Use *cron* syntax for this option. 
You can refer to http://en.wikipedia.org/wiki/Cron for more details, random “R” definition keywords are supported, Vixie cron-style “@” keyword expressions are supported.

Query: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html

"Advanced" button lead you to YAML editor. Please see "Advanced" section for more infromation.

## Advanced
Exports defined in the `Exports` section of the Library and Advanced Exports share common YAML format.

*Advanced export example*

```
define:
  name: Export e-mail
  datasource: elasticsearch

```
** please complete it **

Export files (or advanced exorts) may contain following sections:

#### define
The define section includes the following parameters:

- `name`: The name of the export.
- `datasource`: The name of the DataSource declaration in the Library, specified as a specified as an absolute path to the Library.
- `output`: The output format for the export. Available options are "raw", "csv", and "xlsx" for ES DataSources, and "raw" for Kafka DataSources.
- `header`: When using "csv" or "xlsx" output, you must specify the header of the resulting table as an array of column names. These will appear in the same order as they are listed.
- `schedule`- There are three options how to schedule an export
    - **datetime** in a format YYYY-MM-DD HH:mm (e.g. 2023-01-01 00:00)
    - **timestamp** as integer (e.g. 1674482460)
    - **cron** - you can refer to http://en.wikipedia.org/wiki/Cron for more details, random “R” definition keywords are supported, and remain consistent only within their croniter() instance, Vixie cron-style “@” keyword expressions are supported.


#### target
The target section includes the following parameters:

- `type`: An array of target types for the export. Possible options are "download", "email", and "jupyter". "download" is always selected if the target section is missing.
- `email`: For email target type, you must specify at least the `to` field, which is an array of recipient email addresses. Other optional fields include:
    - `cc`: an array of CC recipients
    - `bcc`: an array of BCC recipients
    - `from`: the sender's email address (string)
    - `subject`: the subject of the email (string)
    - `body`: a file name (with suffix) stored in the Template folder of the library, used as the email body template. You can also add special `parameters` to be used in the template. Otherwise, use any keyword from the define section of your export as a template parameter (for any export it is: `name`, `datasource`, `output`, for specific exports, you can also use parameters. `compression`, `header`, `schedule`, `timezone`, `tenant`).

#### query
The query field must be a string containing an ElasticSearch query object. Please refer to ES documentation: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html


