---
title: SQL - Find any value in any columns in any tables
date: 2018-06-26T04:14:23.623Z
description: SQL Tips & Tricks
---
DECLARE @SearchStr INT = 10810;

\--DECLARE @SearchStr NVARCHAR(100) = 'abc';



CREATE TABLE #Results

\    (

\    ColumnName NVARCHAR(370),

\    ColumnValue INT

\    );



SET NOCOUNT ON;



DECLARE @TableName NVARCHAR(256) = '',

\    @ColumnName NVARCHAR(128)

\--@SearchStr2 NVARCHAR(128) = '%' + @SearchStr + '%'

\    

WHILE @TableName IS NOT NULL

\    BEGIN

\    SET @ColumnName = '';

\    SET @TableName = (SELECT    MIN(QUOTENAME(TABLE_SCHEMA) + '.' + QUOTENAME(TABLE_NAME))

\    FROM      INFORMATION_SCHEMA.TABLES

\    WHERE     TABLE_TYPE = 'BASE TABLE'

\    AND QUOTENAME(TABLE_SCHEMA) + '.' + QUOTENAME(TABLE_NAME) > @TableName

\    AND OBJECTPROPERTY(OBJECT_ID(QUOTENAME(TABLE_SCHEMA) + '.' + QUOTENAME(TABLE_NAME)), 'IsMSShipped') = 0

\    );



\    WHILE (@TableName IS NOT NULL)

\    AND (@ColumnName IS NOT NULL)

\    BEGIN

\    SET @ColumnName = (SELECT   MIN(QUOTENAME(COLUMN_NAME))

\    FROM     INFORMATION_SCHEMA.COLUMNS

\    WHERE    TABLE_SCHEMA = PARSENAME(@TableName, 2)

\    AND TABLE_NAME = PARSENAME(@TableName, 1)

\    AND DATA_TYPE IN ('int')-- ('char', 'varchar', 'nchar', 'nvarchar')

\    AND QUOTENAME(COLUMN_NAME) > @ColumnName

\    );



\    IF @ColumnName IS NOT NULL

\    BEGIN

\    INSERT  INTO #Results

\    EXEC ('SELECT ''' + @TableName + '.' + @ColumnName + ''', LEFT(' + @ColumnName + ', 3630) 

										FROM ' + @TableName + ' (NOLOCK) ' + ' WHERE ' + @ColumnName + ' = ' + @SearchStr --' LIKE ' + @SearchStr2

\    );

\    END;

\    END; 

\    END;



SELECT  ColumnName,

\    ColumnValue

FROM    #Results;

DROP TABLE #Results;
