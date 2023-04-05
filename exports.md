# LogMan.io Export beta

LogMan.io offers a convenient export functionality that enables you to seamlessly transfer log data from the platform to other destinations.
The new Export feature is currently in *beta* and available for use.

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

Here, you have two options: one-off planning and multiple planning. 
- You can plan a single export by clicking “Add schedule” button and selecting a specific date and time for it to be exported.

The format of the date and time must be strictly followed and it goes like this: YYYY-MM-DD HH:mm. That means the date and time should look like this: 2023-01-01 12:00. 
- You can also select to schedule multiple exports on a periodical basis. For this option, use cron syntax format. 

For more details about Cron, you can refer to http://en.wikipedia.org/wiki/Cron.

Generally, the supported keywords are random “R” definition keywords and Vixie cron-style “@” keyword expressions.

After filling in this table, you can go to YAML editor by clicking the “Advanced” button. The “Advanced” section will be explained below.

## Advanced
Exports defined in the Exports section of the Library and Advanced Exports are both in a YAML format. 

Export files (or advanced exports) may contain following sections:

#### Define

The define section includes the following parameters:

- `name`: The name of the export.
- `datasource`: The name of the DataSource declaration in the Library. It should be written as an absolute path that uniquely identifies its location.
- `output`: The format in which you want the exported data to be saved. You can choose from "raw", "csv", and "xlsx" options for Elasticsearch data sources, and "raw" option for Kafka data sources.
- `header`: When exporting data in "csv" or "xlsx" format, you must specify the header of the resulting table as an array of column names. These column names will appear in the same order as they are listed.
- `schedule`- You can choose one of three options for scheduling your export:
    - **datetime** Specify the date and time when you want the export to run in the format YYYY-MM-DD HH:mm (e.g. 2023-01-01 00:00).
    - **timestamp** Specify the Unix timestamp (number of seconds since January 1, 1970) when you want the export to run (e.g. 1674482460).
    - **cron** - Specify a cron expression to schedule the export. You can refer to the Wikipedia page on Cron for more details on how to create cron expressions: http://en.wikipedia.org/wiki/Cron. Random "R" definition keywords are supported, and they remain consistent only within their croniter() instance. Vixie cron-style "@" keyword expressions are also supported.


#### Target

The target section includes the following parameters:

- `type`: Choose from an array of target types for the export. Possible options are "download", "email", and "jupyter". Select "download" everytime the target section is missing.
- `email`: For email target type, you must specify at least the `to` field, which is an array of recipient email addresses. You can also include other optional fields, such as:
    - `cc`: an array of CC recipients
    - `bcc`: an array of BCC recipients
    - `from`: the sender's email address (as a string)
    - `subject`: the subject of the email (as a string)
    - `body`: a file name (with suffix) stored in the Template folder of the library, used as the email body template. You can also add special `parameters` to be used in the template. Otherwise, use any keyword from the define section of your export as a template parameter. For any export, the available template parameters are: name, datasource, and output. For specific exports, you can also use parameters such as compression, header, schedule, timezone, and tenant.

#### Query

The query field must be a string containing an ElasticSearch query object. Please refer to ES documentation: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html

Take a look at an example of an Advaced Export:

```
define:
  name: Export_email_csv_march
  datasource: /DataSources/elasticsearch_short.yaml
  output: csv
  header: [“@timestamp”, “event.dataset”, “event.action”]
  schedule: 2023-03-05 09:52
target:
  type: [“download”]
  email:
  to: "va@teskalabs.com"
  cc: "va@teskalabs.com"
  from: "va@teskalabs.com"
  subject: “March 2023 Logs Export”
  body: “export_email_template.html”
query:
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "event.dataset": "app_logs"
          }
        },
        {
          "match": {
            "event.action": "login"
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "2023-03-01T00:00:00",
              "lte": "2023-03-31T23:59:59"
            }
          }
        }
      ]
    }
  }
}

```

In this example, we have an advanced export named "Export_email_csv_march" that is designed to export data from Elasticsearch. The data will be exported in CSV format, with the column names for the output format defined in the header parameter. The header includes "@timestamp", "event.dataset", and "event.action" columns.

This export is scheduled to run on March 5, 2023 at 9:52 AM. The target type for this Export is "Download", which means that the exported data will be downloaded as a file. However, the exported data will also be sent via email to "va@teskalabs.com", with a copy of the email sent to "va@teskalabs.com". The export will appear as sent from "va@teskalabs.com".

The chosen query for this export will match all events in the "app_logs" dataset that have an "event.action" value of "login" and occurred between March 1 and March 31, 2023 (inclusive). This means that the export will only contain events related to login attempts that occurred during the month of March 2023.
