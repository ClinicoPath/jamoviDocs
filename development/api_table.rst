.. sectionauthor:: Jonathon Love

===================================
Assign results to tables (in ``R``)
===================================

This document describes the properties and methods of a jamovi table
object.

The values of properties can be accessed using the ``$`` operator,
followed by the name. For example, to retrieve the title of a table, one
can go:

.. code-block:: R

   table$title

The methods of a table object are called using the ``$`` operator as
well. For example:

.. code-block:: R

   table$setRow(rowKey=1, values=list(t=2.3, df=2, p=0.45))

Properties
----------

name
~~~~

a string specifying the name of the table

title
~~~~~

a string specifying the title of the table

visible
~~~~~~~

``TRUE`` or ``FALSE``, whether the table is visible or not

status
~~~~~~

a string, one of ``'complete'``, ``'error'``, ``'inited'``, ``'running'``

rowKeys
~~~~~~~

a list of ‘keys’

state
~~~~~

the state

Methods
-------

setStatus(status)
~~~~~~~~~~~~~~~~~

sets the table’s status, should be one of ``'complete'``, ``'error'``,
``'inited'``, ``'running'``

setVisible(visible=TRUE)
~~~~~~~~~~~~~~~~~~~~~~~~

overrides the tables default visibility

setTitle(title)
~~~~~~~~~~~~~~~

sets the table’s title

setError(message)
~~~~~~~~~~~~~~~~~

sets the table’s status to ‘error’, and assigns the error message

setState(object)
~~~~~~~~~~~~~~~~

sets the state object on the table

addColumn(name, …)
~~~~~~~~~~~~~~~~~~

adds a new column to the table, the following arguments are possible:

+------------------+---------+--------------------------------------------+
| argument         | type    | details                                    |
+==================+=========+============================================+
| ``name``         | string  | the name of the column                     |
+------------------+---------+--------------------------------------------+
| ``index``        | integer | the index to insert the column at. if      |
|                  |         | unspecified, the column is appended.       |
+------------------+---------+--------------------------------------------+
| ``title``        | string  | the title to appear at the top of the      |
|                  |         | column. if unspecified, the name is used.  |
+------------------+---------+--------------------------------------------+
| ``superTitle``   | string  | the title to appear above column titles    |
+------------------+---------+--------------------------------------------+
| ``visible``      | |trFl|  | whether the column should be visible. if a |
|                  | or a    | string is specified, this must be a        |
|                  | string  | data-binding to an option.                 |
+------------------+---------+--------------------------------------------+
| ``content``      | string  | either a string that will be placed in     |
|                  |         | every cell, or a data-binding              |
+------------------+---------+--------------------------------------------+
| ``type``         | string  | ‘integer’, ‘number’ or ‘text’; text is     |
|                  |         | left aligned, numbers are right aligned,   |
|                  |         | integers are formatted to zero decimal     |
|                  |         | places                                     |
+------------------+---------+--------------------------------------------+
| ``format``       | string  | a comma separated list of values, such as  |
|                  |         | ‘zto’, ‘pvalue’                            |
+------------------+---------+--------------------------------------------+
| ``combineBelow`` | |trFl|  | if TRUE, when cells in the column are      |
|                  |         | contiguous, and contain the same value,    |
|                  |         | the lower cells will be made blank.        |
+------------------+---------+--------------------------------------------+

addRow(rowKey, values)
~~~~~~~~~~~~~~~~~~~~~~

Adds a row to the table. ``rowKey`` is an object which uniquely
identifies the row – for many cases, simply providing the index is
sufficient. ``values`` is a named list with the values to place in the
cells of that row. The names must correspond to the column names. Not
all column values must be provided, and if a blank row is desired, the
values argument can be omitted entirely.

deleteRows()
~~~~~~~~~~~~

Deletes all the rows in the table

setRow(rowKey, values)
~~~~~~~~~~~~~~~~~~~~~~

Sets the values in an existing row. ``rowKey`` is a key uniquely
identifying the row, and ``values`` is a named list of values. The names
must correspond to the column names. Not all column values need to be
provided.

.. warning :: 
   
   Note that you must explicitly name the rowKey argument:

   .. code-block:: R
   
      setRow(rowKey=...) 
   
   to differentiate from the ``rowNo=...`` argument below

setRow(rowNo, values)
~~~~~~~~~~~~~~~~~~~~~

Sets the values in an existing row. ``rowNo`` is a number specifying the
row number, and ``values`` is a named list of values. The names must
correspond to the column names. Not all column values need to be
provided.

.. warning :: 

   Note that you must explicitly name the rowNo argument when calling
   this method: 

   .. code-block:: R

      setRow(rowNo=...)

   to differentiate from the ``rowKey=...`` argument above

addFormat(rowKey, col, format)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

addFormat(rowNo, col, format)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Adds additional formatting to a cell. ``col`` can be an index or a name.
format can be one of:

-  ``Cell.BEGIN_GROUP``
-  ``Cell.END_GROUP``
-  ``Cell.BEGIN_END_GROUP``
-  ``Cell.NEGATIVE``

Cell.BEGIN_GROUP adds additional padding above the cell. Cell.END_GROUP
adds additional padding below the cell. Cell.NEGATIVE colours the value
red.

setCell(rowKey, col, value)
~~~~~~~~~~~~~~~~~~~~~~~~~~~

setCell(rowNo, col, value)
~~~~~~~~~~~~~~~~~~~~~~~~~~

Sets the value of a cell. Generally setRow() is more efficient.

getCell(rowKey, col)
~~~~~~~~~~~~~~~~~~~~

getCell(rowNo, col)
~~~~~~~~~~~~~~~~~~~

Retrieves a cell.

addFootnote(rowKey, col, note)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

addFootnote(rowNo, col, note)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Adds a footnote to the cell.

addSymbol(rowKey, col, symbol)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

addSymbol(rowNo, col, symbol)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Adds a symbol to a cell – for example an asterisk denoting significance.

setNote(key, note, init=TRUE)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

setNote() adds (or clears) a note placed in the footer of the table.

- ``key``: a string identifying the note
- ``note``: a string representing the text of the note (or NULL)
- ``init``: whether this be considered an ``init`` note

Specifying a ``note`` of NULL causes the note to be removed.

``init`` notes are those that are added during the *init* phase.
``init`` notes are typically based on the values of the options. For
example, if the user has specified an alternative hypothesis — that
population one is greater than population two — the analysis could add a
note indicating this. In contrast, ``non-init`` notes are created in the
*run* phase. An example might be the number of subjects that were
excluded from the analysis as a result of containing missing values.
``init`` notes are typically based on the values of options, where as
``non-init`` notes depend on the data.

In practice, when an analysis is changed or re-run, ``init`` notes are
not restored from state; they are simply recreated during the ``init``
phase. In contrast, ``non-init`` notes are restored from state.

Note that if the text of the note will always be the same, it is
recommended to set the note in the ``.r.yaml`` file instead.

.. --------------------------------------------------------------------

.. |trFl|    replace:: ``TRUE`` / ``FALSE``
