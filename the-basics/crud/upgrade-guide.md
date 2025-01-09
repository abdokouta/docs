# Upgrade Guide

## Version 3

changed how `$filterFields` array works. it no longer renames fields, instead, it restricts filters that are accepted by the field as mentioned in the Restrict filters section. to rename fields refer to [Rename fields](broken-reference). `sortFields` However, didnt change.

## Version 2

changed filter function arguments. filter function no longer accepts filter methods, instead, filter function now accepts filter source as mentioned in Custom Filters section. to specify allowed filter methods use filterBy as mentioned in [Restrict Filters](broken-reference)
