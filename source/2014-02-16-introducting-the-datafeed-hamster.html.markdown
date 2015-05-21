---
title: Introducing the Datafeed Hamster
date: 2014-02-16
tags: Adobe Analytics, data feed, rake, ruby on rails, SiteCatalyst
---

Why make the Hamster?

We have worked with several clients who have needed to use the Data Feeds from Adobe Marketing Suite > Analytics (used to be SiteCatalyst) for exploratory analysis, debugging, or customer segmentation. For the uninitiated, the data feed is the data collected from each beacon sent in to the Adobe servers. As a result, the data feed can be difficult to work with. To process these data feeds we created the Datafeed Hamster.

Data feeds usually have two of the three problem areas for data: complexity and quality. First, the data feed is complex in its structure. For the version as of the writing of this post, there are 474 columns. Second, the data feed can suffer from severe data quality issues. Because this is the raw data that was sent in there are often values that cause problems when loading into a database.

For example, user-entered values are often captured from forms or CMS systems that contain problem characters like tabs, commas, back slashes, double quotes, or single quotes. Another example is link handler plugins capturing javascript functions. Both of these situations create rows in the data feed that are difficult to process.

For some clients, data feeds also get very large and move into the data problem area; volume. We have used the Hamster to process 2 Gb daily files, it can definitely handle more.

How to run the Hamster?

Right now, the Hamster is a rake task in a thin Ruby on Rails app using the MySQL2 gem. To get the hamster just clone the git repo with:

git clone https://github.com/analyticsPierce/datafeed-hamster.git
To get it setup, go to /lib/tasks/datafeed_process.rake and set the credentials and endpoint for the @db MySQL connection. You will need to copy the hit_data.tsv files into the /data folder off of Rails.root. Once the files are processed, they will be archived into /data/archive.

datafeed_hamster_2
http://marketingscience.co/wp-content/uploads/2014/02/datafeed_hamster_2-300x44.png

Before you start running, you will need to update the connection info in database.yml and run all the migrations. So $rake db:migrate.

At a high level, the process uses a double encoding method along with some character scrubbing to remove problem characters. Rows that cannot be loaded are captured to the log and skipped. With the most recent files we ran, the error rate was < 5%. Individual rows are loaded into the database so it can process large files without difficulty.

The rake task can be initiated from the Rails app with: $rake datafeed:load. All .tsv files in the /data folder will be processed.

datafeed_hamster_1
http://marketingscience.co/wp-content/uploads/2014/02/datafeed_hamster_1-300x54.png
 

 

Please take the Hamster for a run and let me know how it goes. If you have suggestions please fork and send a PR.

We are making some upgrades to this process now to add it to the MIND Analytics Platform. In addition, we are working on a MapReduce version. From our experience, loading the several datafeeds into a relational database is better for exploring the data but productionalizing large datafeeds benefits from using tools like Elastic MapReduce, especially if you are only capturing specific segments or fields.
