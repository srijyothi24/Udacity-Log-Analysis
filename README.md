# Udacity-Log-Analysis

### Project Overview
>In this project, you'll work with data that could have come from a real-world web application, with fields representing information that a web server would record, such as HTTP status codes and URL paths. The web server and the reporting tool both connect to the same database, allowing information to flow from the web server into the report.

### Installation and Execution
This project makes use of the Linux-based virtual machine (VM). This will give you the PostgreSQL database and support software needed for this project, use tools called Vagrant and VirtualBox to install and manage the VM. VirtualBox is the software that actually runs the virtual machine download it https://www.virtualbox.org/wiki/Downloads
Vagrant is the software that configures the VM and lets you share files between your host computer and the VM's filesystem download it https://www.vagrantup.com/downloads.html
  
#### Launching the Virtual Machine:
  1. Launch the Vagrant VM inside Vagrant sub-directory in the downloaded fullstack-nanodegree-vm repository using command:
  
  ```
    $ vagrant up
  ```
  2. Then Log into this using command:
  
  ```
    $ vagrant ssh
  ```
  3. Change directory to /vagrant and look around with ls.
  
#### Setting up the database and Creating Views:

  1. Load the data in local database using the command:
  
  ```
    psql -d news -f newsdata.sql
  ```
  The database includes three tables:
  * The authors table includes information about the authors of articles.
  * The articles table includes the articles themselves.
  * The log table includes one entry for each time a user has accessed the site.
  
  2. Use `psql -d news` to connect to database.
  
  3. Create view article_view using:
  ```
    create view article_view as select title,author,count(*) as views from articles,log where 
    log.path like concat('%',articles.slug) group by articles.title,articles.author 
    order by views desc;
  ```
  | Column  | Type    |
  | :-------| :-------|
  | title   | text    |
  | author  | text    |
  | views   | Integer |
  
  4. Create vier error_log_view using:
  ```
    create view error_log_view as select date(time),round(100.0*sum(case log.status when '200 OK' 
    then 0 else 1 end)/count(log.status),2) as "Percent Error" from log group by date(time) 
    order by "Percent Error" desc;
  ```
  | Column        | Type    |
  | :-------      | :-------|
  | date          | date    |
  | Percent Error | float   |
  
#### Running the queries:
  1. From the vagrant directory inside the virtual machine,run logs.py using:
  ```
    $ python3 logs.py
  ```
