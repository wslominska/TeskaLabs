# LogMan.io Export beta

LogMan.io offers a convenient export functionality that enables you to seamlessly transfer log data from the platform to other destinations.
the new Export feature is currently in *beta* and available for use
With this feature, you can effortlessly create, run, remove, or download exports as needed. 
You can find a detailed explanation of the Export functionality below.

## List of Exports
The list of Exports allows you to see all the exports with the information about their data source, output, export size, and when it was exported. The exports on the list are sorted according to their export time: from the most recent to the oldest. You can open each export’s detailed view by clicking on its name.
The exports on the list are categorized according to their colour: 
- Exports in green indicate successfully completed exports. 
- Exports in yellow indicate exports that are in progress.
- Exports in red indicate failed exports.
- Exports in blue indicate scheduled exports.

Each export can be downloaded or deleted.

You can search the list by typing the name of the export in the search bar. On that page, you can also make a new export by clicking ‘New’ in the upper right corner.

## Export Detail
You can access detailed information about each export on the Detail screen. On that screen, you can also download and delete the export.

By clicking ‘Restart’, you can run the export again – whether as it is, or with the custom changes you implemented. 

If you delete a scheduled export, the exports that were generated from it before do not get removed. 


## Start Export
By clicking on the Start Export screen, you can access pre-made Exports from the Library. You can either run them as they are or modify them and run them with the custom modifications you made. 

Do you want to create your own export?
You can do that by clicking the “Custom” button. It will take you to a blank form that you can use for your self-made export.

## Custom Export
If you want to create your own, custom export, follow these steps:

1.	Select the Data Source. By making this choice, the other export options will be determined. 

2.	Select the Output. 
 - If you select CSV output, you need to enter the column names. You can add more columns by clicking “Add new header” in the section below. The order of the columns can be changed by dragging. The order you will arrange the columns in will stay the same in the resulting table. Remember that if you leave the columns blank or the columns’ names will not match the search terms in the database, the resulting table will be empty. 

3.	Select the Target. 

4.	This selection depends on the configuration of the Export function and can include “e-mail” or “jupyter” targets.
 - If you choose “e-mail”, you can choose a predefined e-mail template form the Library or create your own. 
 - If you choose ‘jupyter” the files will be exported directly to jupyter notebook integrated within LogMan.io.

5.	Select separator

6.	Add schedule


## Advanced
Exports defined in the `Exports` section of the Library and Advanced Exports share common YAML format.

*Advanced export example*

```
define:
  name: Export e-mail
  datasource: elasticsearch

```
** please complete it **

Export files (or advanced exports) may contain following sections:

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


