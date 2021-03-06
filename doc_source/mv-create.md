# CREATE MATERIALIZED VIEW<a name="mv-create"></a>


|  | 
| --- |
| This is prerelease documentation for the materialized view feature for Amazon Redshift, which is in preview release\. The documentation and the feature are both subject to change\. We recommend that you use this feature only with test clusters, and not in production environments\. For preview terms and conditions, see Beta Service Participation in [AWS Service Terms](https://aws.amazon.com/service-terms/)\.   | 

Creates a materialized view

## Syntax<a name="mv_CREATE_MATERIALIZED_VIEW-synopsis"></a>

```
CREATE MATERIALIZED VIEW mv_name
[ BACKUP { YES | NO } ]
[ table_attributes ]   
AS query
```

## Parameters<a name="mv_CREATE_MATERIALIZED_VIEW-parameters"></a>

BACKUP  
Specifies whether the materialized view is included in automated and manual cluster snapshots, which are stored in Amazon S3\.  
The default value for `BACKUP` is `YES`\.  
You can specify `BACKUP NO` to save processing time when creating snapshots and restoring from snapshots, and to reduce the amount of storage required in Amazon S3\.  
The `BACKUP NO` setting has no effect on automatic replication of data to other nodes within the cluster, so tables with BACKUP NO specified are restored in a node failure\.

 *table\_attributes*   
Specifies how the data in the materialized view is distributed:  
+ `DISTSTYLE { EVEN | ALL | KEY }` is the distribution style for the materialized view\. If you omit this clause, the distribution style is `AUTO`\. For more information, see [Distribution Styles](c_choosing_dist_sort.md)\.
+ `DISTKEY ( distkey_identifier )` is the distribution key for the materialized view\. For more information, see [Designating Distribution Styles](t_designating_distribution_styles.md)\.
+ `SORTKEY ( column_name [, ...] )` is the sort key for the materialized view\. For more information, see [Choosing Sort Keys](t_Sorting_data.md)\.

AS *query*  
A valid `SELECT` statement, subject to limitations\. For more information about limitations, see [Limitations and Usage Notes](mv-usage-notes.md)\. The result set from the query defines the columns and rows of the materialized view\. 

## Usage Notes<a name="mv_CREATE_MARTERIALIZED_VIEW_usage"></a>

In order to create a materialized view, you must have `CREATE` privileges in a schema\. 

## Example<a name="mv_CREATE_MARTERIALIZED_VIEW_examples"></a>

The following example creates a materialized view\. Each row represents a category, along with the number of tickets sold or unsold for that category\.

```
CREATE MATERIALIZED VIEW tickets_mv AS
    select   catgroup,
    sum(qtysold) as sold
    from     category c, event e, sales s
    where    c.catid = e.catid
    and      e.eventid = s.eventid
    group by catgroup;
```