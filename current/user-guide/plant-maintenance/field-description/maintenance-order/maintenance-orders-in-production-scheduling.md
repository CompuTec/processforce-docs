# Maintenance Orders in Production Scheduling

Maintenance Orders (Status: Scheduled, Released, Started; MI Excluded = Yes) are considered in Production Scheduling and are visible on Gantt Chart as non-available working time.

MIs Parent-Child dependency is used if for child Dependent = Yes. In such a case, if MO is generated for the parent, the parent and his child are considered as not accessible for production.
