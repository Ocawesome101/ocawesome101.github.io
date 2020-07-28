*NAME*
        initsvc - API for starting and stopping system daemons

*DESCRIPTION
        initsvc* is provided by the *InitMe* init system as a method of controlling system services.  Starting and stopping services require root access.

        Configuration is storeed in */etc/initsvc.cfg*.

*METHODS
        start*(*service*:*string*[, *handler*:*function*]): *boolean* or *nil*, *string*
                Start service *service*.  Looks for */lib/services/*service*.lua*.

        *list*(): *table*
                Returns a table of services, and whether they are running.  Each entry is a sub-table with the fields *name*:*string* and *running*:*boolean*.
        
        *stop*(*service*:*string*): *boolean* or *nil*, *string*
                Attempts to stop service *service*.  May have compatibility with OpenOS services.

        *enable*(*service*:*string*): *boolean* or *nil*, *string*
                Attempts to enable service *service*.

        *disable*(*service*:*string*)
                Disable service *script*.

*NOTES*
        [Until 2020.7.9]  Due to the manner in which *initsvc*'s configuration is formatted, a script and a service cannot have the same name.  This may change in the future.

        [2020.7.9]  This has been fixed.  Startup scripts are now run in order, before services, from */lib/scripts/*.

*COPYRIGHT
        Monolith System Interfaces* (c) 2020 Ocawesome101 under the GNU GPLv3.

*SEE ALSO
        init*(*1*), *init*(*5*)
