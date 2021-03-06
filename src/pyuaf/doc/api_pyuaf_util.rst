``pyuaf.util``
====================================================================================================


.. currentmodule:: pyuaf.util


.. automodule:: pyuaf.util


**SUMMARY of submodules:**

.. autosummary:: 
    
        applicationtypes
        attributeids
        browsedirections
        constants
        errors
        loglevels
        messagesecuritymodes
        monitoringmodes
        nodeclasses
        nodeididentifiertypes
        opcuaidentifiers
        opcuastatuscodes
        opcuatypes
        primitives
        securitypolicies
        statuscodes
        timestampstoreturn
        usertokentypes
        
**SUMMARY of classes:**
        
        See sidebar.


*class* Address
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.Address

    An Address points to a node within an server address space by means of an
    absolute address (an ExpandedNodeId) or by means of a relative address (a path relative to
    another address).
    
    An "address" is a term defined by the UAF, and *not* by the OPC UA standard. It's basically
    a union class that holds the information to identify a node within an address space.
    Several addresses can all point to the same node.
    
    A relative address may point to other relative addresses. More formally, the starting
    point of an Address based on a relative path, may also be an Address based on a 
    relative path in turn. This way you can define large and complex hierarchies of relative 
    addresses, with just a single absolute address (based on an ExpandedNodeId) as the starting 
    point.


    * Methods:

        .. automethod:: pyuaf.util.Address.__init__
    
            Construct a new Address by providing either
            
               * an :class:`~pyuaf.util.ExpandedNodeId`
               
               * another :class:`~pyuaf.util.Address` (the starting point) and a 
                 :class:`~pyuaf.util.RelativePath` (the path).
            
            Usage example:
            
            .. doctest::
            
                >>> import pyuaf
                >>> from pyuaf.util import Address, NodeId, ExpandedNodeId, RelativePathElement, QualifiedName, BrowsePath
                
                >>> # the following ways of constructing a Address are possible:
                >>> address0 = Address( ExpandedNodeId("someId", "someNsUri", "someServerUri") )
                >>> address1 = Address( NodeId("someId", "someNsUri"), "someServerUri" )
                >>> address2 = Address( address0, [RelativePathElement(QualifiedName("someName", "someNsUri"))] )
                >>> address3 = Address( address1, [RelativePathElement(QualifiedName("someOtherName", 3))] )
                >>> address3 = Address( BrowsePath( ExpandedNodeId("someId", "someNsUri", "someServerUri"), 
                ...                                 [RelativePathElement(QualifiedName("someOtherName", 3))] ) )
                
                >>> # you may also provide a single RelativePathElement in case the relative path contains just
                >>> # one element:
                >>> address4 = Address( address1, RelativePathElement(QualifiedName("someOtherNameToo", 3)) )
            
            
            
        .. automethod:: pyuaf.util.Address.isExpandedNodeId
        
            Check if the address is defined as an ExpandedNodeId.
            
            :return: True if the address is based on an :class:`~pyuaf.util.ExpandedNodeId` 
                     (and you're allowed to call :meth:`~pyuaf.util.Address.getExpandedNodeId`), False if not.
            :rtype:  bool


        .. automethod:: pyuaf.util.Address.isRelativePath
        
            Check if the address is defined as a RelativePath.
            
            :return: True if the address is based on a :class:`~pyuaf.util.RelativePath`
                     and another :class:`~pyuaf.util.Address`  
                     (and you're allowed to call :meth:`~pyuaf.util.Address.getStartingAddress` 
                     and :meth:`~pyuaf.util.Address.getRelativePath`), False if not.
            :rtype:  bool

            
        .. automethod:: pyuaf.util.Address.getExpandedNodeId
        
            Get the ExpandedNodeId in case :meth:`~pyuaf.util.Address.isExpandedNodeId` returns True.
            
            :return: The ExpandedNodeId corresponding with this Address.
            :rtype:  :class:`~pyuaf.util.ExpandedNodeId`

            
        .. automethod:: pyuaf.util.Address.getRelativePath
        
            Get the RelativePath in case :meth:`~pyuaf.util.Address.isRelativePath` returns True.
            
            :return: The RelativePath corresponding with this Address, as a ``tuple`` 
                     of :class:`~pyuaf.util.RelativePathElement`.

            
        .. automethod:: pyuaf.util.Address.getStartingAddress
        
            Get a pointer (not a copy of the actual object itself!) to the starting address in case 
            :meth:`~pyuaf.util.Address.isRelativePath` returns True.
            
            :return: A reference to the starting address.
            :rtype:  :class:`~pyuaf.util.Address`




*class* ApplicationDescription
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ApplicationDescription

    An ApplicationDescription instance describes a Server or Client or ClientAndServer, or a
    DiscoveryServer.


    * Methods:

        .. automethod:: pyuaf.util.ApplicationDescription.__init__
    
            Construct a new ApplicationDescription.
    
        .. automethod:: pyuaf.util.ApplicationDescription.isEmpty
    
            Is this a valid application description or an empty one?
            
            :rtype: ``bool``


    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.ApplicationDescription.applicationUri
        
            The application URI as a string (type ``str``).
    
        .. autoattribute:: pyuaf.util.ApplicationDescription.productUri
        
            The product URI as a string (type ``str``).
    
        .. autoattribute:: pyuaf.util.ApplicationDescription.applicationName
        
            The application name as a localized text (type :class:`~pyuaf.util.LocalizedText`).
    
        .. autoattribute:: pyuaf.util.ApplicationDescription.applicationType
        
            The application type as an int (as defined in :mod:`pyuaf.util.applicationtypes`).
    
        .. autoattribute:: pyuaf.util.ApplicationDescription.gatewayServerUri
        
             The gateway server URI as a string (type ``str``).
    
        .. autoattribute:: pyuaf.util.ApplicationDescription.discoveryProfileUri
        
             The discovery profile URI as a string (type ``str``).
    
        .. autoattribute:: pyuaf.util.ApplicationDescription.discoveryUrls
        
             The discovery URLs as a list of strings (type :class:`~pyuaf.util.StringVector`).
    



*class* ApplicationDescriptionVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ApplicationDescriptionVector

    An ApplicationDescriptionVector is a container that holds elements of type 
    :class:`pyuaf.util.ApplicationDescription`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`~pyuaf.util.ApplicationDescription`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import ApplicationDescriptionVector, ApplicationDescription
        
        >>> # construct a ApplicationDescriptionVector without elements:
        >>> descriptions = ApplicationDescriptionVector()
        
        >>> noOfElements = len(descriptions) # will be 0
        
        >>> descriptions.append(ApplicationDescription())
        
        >>> noOfElements = len(descriptions) # will be 1
        
        >>> descriptions.resize(4)
        
        >>> noOfElements = len(descriptions) # will be 4
        
        >>> description0 = descriptions[0]
        
        >>> # you may construct an ApplicationDescriptionVector from a regular Python list:
        >>> otherDescriptions = ApplicationDescriptionVector( [ApplicationDescription(), ApplicationDescription()] )


*class* BrowsePath
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.BrowsePath

    A BrowsePath holds a path from a resolved starting node (an ExpandedNodeId) to another node 
    within the address space.


    * Methods:

        .. method:: __init__([startingExpandedNodeId [, relativePath]])
    
            Construct a new BrowsePath.
            
            You may optionally provide a starting point for the browse path 
            (a :class:`pyuaf.util.ExpandedNodeId`) and optionally provide a relative path
            (which is a :class:`~pyuaf.util.RelativePath`, resembling a ``list`` of 
            :class:`~pyuaf.util.RelativePathElement`).


    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.BrowsePath.startingExpandedNodeId
        
            The starting point (as an :class:`~pyuaf.util.ExpandedNodeId`)) of the relative path.
    
        .. autoattribute:: pyuaf.util.BrowsePath.relativePath
        
             The relative path as a list of qualified names 
             The type of this attribute is :class:`~pyuaf.util.RelativePath`, 
             which resembles a ``list`` of :class:`~pyuaf.util.RelativePathElement`.


*class* ByteStringVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ByteStringVector

    An ByteStringVector is a container that holds elements of the built-in Python type 
    ``bytearray``.
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of ``bytearray``.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import ByteStringVector
        
        >>> # construct a ByteStringVector without elements:
        >>> vec = ByteStringVector()
        
        >>> noOfElements = len(vec) # will be 0
        
        >>> vec.append(bytearray("abcd"))
        
        >>> noOfElements = len(vec) # will be 1
        
        >>> vec.resize(4)
        
        >>> noOfElements = len(vec) # will be 4
        
        >>> # vec[0] was already assigned to bytearray("abcd")
        >>> vec[1] = bytearray("efgh")
        >>> vec[2] = bytearray("ijkl")
        >>> vec[3] = bytearray("mnop")
        
    

*class* DataChangeFilter
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.DataChangeFilter

    A DataChangeFilter specifies a deadband filter for monitored data items.
    
    
    * Methods:

        .. automethod:: pyuaf.util.DataChangeFilter.__init__
            
            Create a default DataChangeFilter instance.
            
            Defaults:
              
               - :attr:`~pyuaf.util.DataChangeFilter.trigger`: :attr:`~pyuaf.util.DataChangeFilter.DataChangeTrigger_StatusValue`
               - :attr:`~pyuaf.util.DataChangeFilter.deadBandType`: :attr:`~pyuaf.util.DataChangeFilter.DeadBandType_None`
               - :attr:`~pyuaf.util.DataChangeFilter.deadBandValue`: 0.0

        .. automethod:: pyuaf.util.DataChangeFilter.__str__
            
            Get a string representation.


    * Class attributes:

        * for the dead band type:

            .. autoattribute:: pyuaf.util.DataChangeFilter.DeadBandType_None
    
                An ``int`` identifying the kind of deadband: in this case no deadband at all!
    
            .. autoattribute:: pyuaf.util.DataChangeFilter.DeadBandType_Absolute
    
                An ``int`` identifying the kind of deadband: in this case an absolute deadband.
    
            .. autoattribute:: pyuaf.util.DataChangeFilter.DeadBandType_Percent
    
                An ``int`` identifying the kind of deadband: in this case a relative deadband 
                (in percent).

        * for the data change trigger:

            .. autoattribute:: pyuaf.util.DataChangeFilter.DataChangeTrigger_Status
    
                An ``int`` identifying the kind of trigger: in this case trigger when the status 
                changes!
    
            .. autoattribute:: pyuaf.util.DataChangeFilter.DataChangeTrigger_StatusValue
    
                An ``int`` identifying the kind of trigger: in this case trigger when the status 
                or the value changes!
    
            .. autoattribute:: pyuaf.util.DataChangeFilter.DataChangeTrigger_StatusValueTimestamp
    
                An ``int`` identifying the kind of trigger: in this case trigger when the status 
                or the value or the timestamp changes!


    * Attributes:
    
        .. autoattribute:: pyuaf.util.DataChangeFilter.trigger
        
            The kind of trigger, as one of these ``int`` values:
            :attr:`~pyuaf.util.DataChangeFilter.DataChangeTrigger_Status` 
            or :attr:`~pyuaf.util.DataChangeFilter.DataChangeTrigger_StatusValue` 
            or :attr:`~pyuaf.util.DataChangeFilter.DataChangeTrigger_StatusValueTimestamp`.
    
        .. autoattribute:: pyuaf.util.DataChangeFilter.deadBandType
        
            The type of dead band, as one of these ``int`` values:
            :attr:`~pyuaf.util.DataChangeFilter.DeadBandType_None` 
            or :attr:`~pyuaf.util.DataChangeFilter.DeadBandType_Absolute` 
            or :attr:`~pyuaf.util.DataChangeFilter.DeadBandType_Percent`.

        .. autoattribute:: pyuaf.util.DataChangeFilter.deadBandValue
        
            Value of the deadband, as a ``float``.





*class* DataValue
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.DataValue

    A DataValue holds some data value, a status, timestamps, ...


    * Methods:

        .. automethod:: pyuaf.util.DataValue.__init__
    
            Construct a new DataValue.
    
        .. automethod:: pyuaf.util.DataValue.__str__
    
            Get a string representation
            
            :rtype: ``str``


    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.DataValue.data
        
            The data, as one of the data types described in :ref:`note-variants`.
  
        .. autoattribute:: pyuaf.util.DataValue.opcUaStatusCode
        
            The OPC UA status code (see specs part 4) of the data, as an ``int``.
  
        .. autoattribute:: pyuaf.util.DataValue.sourceTimestamp
        
            The source time stamp of the data, as a :class:`~pyuaf.util.DateTime` instance.
  
        .. autoattribute:: pyuaf.util.DataValue.serverTimestamp
        
            The server time stamp of the data, as a :class:`~pyuaf.util.DateTime` instance.
  
        .. autoattribute:: pyuaf.util.DataValue.sourcePicoseconds
    
            The number of 10 picosecond intervals that need to be added to the source timestamp
            (to get a higher time resolution), as an ``int``.
  
        .. autoattribute:: pyuaf.util.DataValue.serverPicoseconds
    
            The number of 10 picosecond intervals that need to be added to the server timestamp
            (to get a higher time resolution), as an ``int``.
    



*class* DataValueVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.DataValueVector

    An DataValueVector is a container that holds elements of type 
    :class:`pyuaf.util.DataValue`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`pyuaf.util.DataValue`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import DataValueVector, DataValue
        >>> from pyuaf.util import NodeId, LocalizedText
        >>> from pyuaf.util import primitives
        
        >>> # construct a DataValueVector without elements:
        >>> vec = DataValueVector()
        
        >>> noOfElements = len(vec) # will be 0
        
        >>> vec.append( DataValue(primitives.UInt32(1234)) )
        
        >>> noOfElements = len(vec) # will be 1
        
        >>> vec.resize(4)
        
        >>> noOfElements = len(vec) # will be 4
        
        >>> # vec[0] was already assigned to an UInt32
        >>> vec[1].data = primitives.String("some text")
        >>> vec[2].data = NodeId("SomeId", "SomeNsUri")
        >>> vec[3].data = LocalizedText("en", "Some English Text")
        
        >>> # you may construct an DataValueVector from a regular Python list:
        >>> otherVec = DataValueVector( [ DataValue(), DataValue(primitives.Boolean(True))] )
        
    


*class* DateTime
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.DateTime


    An DateTime holds a timestamp.
    
    Short example:

    .. doctest::
    
        >>> import pyuaf
        >>> import time, datetime
        >>> from pyuaf.util import DateTime
        
        >>> nowDateTime = DateTime.now()  
        >>> nowDateTime2 = DateTime(time.time()) # equally valid alternative
    
        >>> nowStringRepresentation = str(nowDateTime)
        
        >>> # create a time in the future (note that 567 milliseconds have been added on the last line!)
        >>> futureTimeFloat = time.mktime(datetime.datetime(year   = 2023, 
        ...                                                 month  =    6, 
        ...                                                 day    =   18, 
        ...                                                 hour   =    9, 
        ...                                                 minute =   12, 
        ...                                                 second =   34).timetuple()) + 0.567
        >>> futureDateTime = DateTime(futureTimeFloat)
        
        >>> # equally valid alternative:
        >>> futureDateTime2 = DateTime()
        >>> futureDateTime2.setCtime(futureTimeFloat)
        
        >>> # calculate the number of days difference between "now" and the "future"
        >>> daysDifference = nowDateTime.daysTo(futureDateTime)
        

    * Methods:

        .. automethod:: pyuaf.util.DateTime.__init__
    
            You can construct a pyuaf.util.DateTime instance by providing a floating point number
            which represents the number of seconds (+ milliseconds after the comma) since
            UTC 1 January 1970 (= 1970-01-01T00:00:00Z).
            
            
        .. automethod:: pyuaf.util.DateTime.isNull
        
            True if the datetime is NULL, False if it has a real value.
            
            :return: True if the datetime is NULL, false if it has a real value.
            :rtype: ``bool``
             
        .. automethod:: pyuaf.util.DateTime.toFileTime
        
            Get the time as a FILETIME, i.e. as a 64-bit integer corresponding to the number
            of 100-nanosecond intervals since January 1, 1601 UTC).

            :return: The time as a 64-bit number.
            :rtype: ``long``
            
            
        .. automethod:: pyuaf.util.DateTime.__str__
        
            Get the time as a string representation, e.g. "2013-05-21T12:34:56.789Z".
           
            :return: The time as a string.
            :rtype: ``str``
            
            
        .. automethod:: pyuaf.util.DateTime.toDateString
        
            Get the date part as a string representation, e.g. "2013-05-21".
           
            :return: The date as a string.
            :rtype: ``str``
            
            
        .. automethod:: pyuaf.util.DateTime.toTimeString
        
            Get the time part as a string representation, e.g. "12:34:56.789Z".
           
            :return: The time without the date, as a string.
            :rtype: ``str``
            
            
        .. automethod:: pyuaf.util.DateTime.toTime_t
        
            Get the time part as the number of seconds since UTC 1 January 1970
            (= 1970-01-01T00:00:00Z).
           
            :return: The time as an integer.
            :rtype: ``int``
            
            
        .. automethod:: pyuaf.util.DateTime.setCtime(t)
        
            Update the time to the given floating point value, corresponding 
            to the number of seconds (+ milliseconds, after the comma) since UTC 1 January 1970
            (= 1970-01-01T00:00:00Z).
           
            :param t: The time , e.g. ``time.time()`` for the current time.
            :type t: ``float``
            :return: The time as a :class:`~pyuaf.util.DateTime` object.
            :rtype:  :class:`~pyuaf.util.DateTime`
            
            
        .. automethod:: pyuaf.util.DateTime.msec
        
            Get the milliseconds part of the timestamp.
           
            :return: The number of milliseconds after the second.
            :rtype: ``int``
            
            
        .. automethod:: pyuaf.util.DateTime.daysTo(other)
        
            Get the number of days from this instance to the argument instance.
           
            :param other: Another DateTime instance.
            :type other:  :class:`~pyuaf.util.DateTime`
            :return: The number of days difference.
            :rtype: ``int``
            
            
        .. automethod:: pyuaf.util.DateTime.secsTo(other)
        
            Get the number of seconds from this instance to the argument instance.
           
            :param other: Another DateTime instance.
            :type other:  :class:`~pyuaf.util.DateTime`
            :return: The number of seconds difference.
            :rtype: ``int``
            
            
        .. automethod:: pyuaf.util.DateTime.msecsTo(other)
        
            Get the number of milliseconds from this instance to the argument instance.
           
            :param other: Another DateTime instance.
            :type other:  :class:`~pyuaf.util.DateTime`
            :return: The number of milliseconds difference.
            :rtype: ``int``
            
            
        .. automethod:: pyuaf.util.DateTime.addSecs(secs)
        
            Add a number of seconds to the timestamp.
           
            :param secs: Number of seconds to add.
            :type secs:  ``int``
            
            
        .. automethod:: pyuaf.util.DateTime.addMilliSecs(msecs)
        
            Add a number of milliseconds to the timestamp.
           
            :param msecs: Number of milliseconds to add.
            :type msecs:  ``int``


    * Static functions:
            
            
        .. automethod:: pyuaf.util.DateTime.now
        
            Static function to get the current time.
           
            :return: The current time.
            :rtype:  :class:`~pyuaf.util.DateTime`


        .. automethod:: pyuaf.util.DateTime.fromString(s)
        
            Static function to convert the given string into a DateTime object.
           
            :param s: The string, e.g. "2013-05-21T12:34:56.789Z".
            :type s: ``str``
            :return: The time from the string.
            :rtype:  :class:`~pyuaf.util.DateTime`
            
            
        .. automethod:: pyuaf.util.DateTime.fromTime_t(t)
        
            Static function to convert the given time (as an integer corresponding to 
            ``long(time.time())``) into a DateTime object.
           
            This time ("time_t") is the number of seconds since UTC 1 January 1970 
            = 1970-01-01T00:00:00Z.
           
            :param t: The time , e.g. ``long(time.time()) + 3`` for the current time + 3 seconds.
            :type t: ``long``
            :return: The time as a :class:`~pyuaf.util.DateTime` object.
            :rtype:  :class:`~pyuaf.util.DateTime`
            
            
        .. automethod:: pyuaf.util.DateTime.fromFileTime(t)
        
            Static function to convert the given time (as a 64-bit integer corresponding to 
            FILETIME (= the number of 100-nanosecond intervals since January 1, 1601 UTC).
           
            :param t: The FILETIME.
            :type t: ``long``
            :return: The time as a :class:`~pyuaf.util.DateTime` object.
            :rtype:  :class:`~pyuaf.util.DateTime`
            
            
        .. automethod:: pyuaf.util.DateTime.sleep(secs)
        
            Static function to sleep a number of seconds.
           
            :param secs: Number of seconds to sleep.
            :type secs:  ``int``
            
            
        .. automethod:: pyuaf.util.DateTime.msleep(msecs)
        
            Static function to sleep a number of milliseconds.
           
            :param msecs: Number of milliseconds to sleep.
            :type msecs:  ``int``
            


*class* DateTimeVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.DateTimeVector

    A DateTimeVector is a container that holds elements of type 
    :class:`pyuaf.util.DateTime`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`~pyuaf.util.DateTime`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> import time, datetime
        >>> from pyuaf.util import DateTimeVector, DateTime
        
        >>> # construct a DateTimeVector without elements:
        >>> vec = DateTimeVector()
        
        >>> noOfElements = len(vec) # will be 0
        
        >>> vec.append(DateTime(time.time()))
        
        >>> noOfElements = len(vec) # will be 1
        
        >>> vec.resize(2)
        
        >>> vec[1].setCtime(time.time())
        >>> vec[1].addSecs(3)
        
        >>> noOfElements = len(vec) # will be 2
        
        >>> # you may construct a DateTimeVector from a regular Python list:
        >>> fromTime = time.mktime(datetime.datetime(year   = 2013, 
        ...                                          month  =    6, 
        ...                                          day    =   18, 
        ...                                          hour   =    9, 
        ...                                          minute =   12, 
        ...                                          second =   34).timetuple()) + 0.567 # 567 msec 
        >>> toTime = time.mktime(datetime.datetime(year   = 2014, 
        ...                                        month  =    1, 
        ...                                        day    =   30, 
        ...                                        hour   =   12, 
        ...                                        minute =   34, 
        ...                                        second =   56).timetuple()) + 0.789 # 789 msec 
        >>> someOtherVec = DateTimeVector( [ DateTime(fromTime), DateTime(toTime) ] )





*class* EndpointDescription
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.EndpointDescription

    An EndpointDescription instance describes an endpoint of a Server (as returned during the
    discovery process, so that the client can choose an endpoint and connect to it).


    * Methods:

        .. automethod:: pyuaf.util.EndpointDescription.__init__
    
            Construct a new EndpointDescription.
    
        .. automethod:: pyuaf.util.EndpointDescription.isEmpty
    
            Is this a valid endpoint description or an empty one?
            
            :rtype: ``bool``


    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.EndpointDescription.endpointUrl
        
            The endpoint URL (type ``str``).
    
        .. autoattribute:: pyuaf.util.EndpointDescription.server
        
            The application description of the server (type :class:`~pyuaf.util.ApplicationDescription`).
    
        .. autoattribute:: pyuaf.util.EndpointDescription.serverCertificate
        
            The server certificate data (type ``bytearray``).
    
        .. autoattribute:: pyuaf.util.EndpointDescription.securityMode
        
            The message security mode (e.g. :attr:`pyuaf.util.messagesecuritymodes.Mode_SignAndEncrypt`)
            (an ``int``, as defined in :mod:`pyuaf.util.messagesecuritymodes`).
    
        .. autoattribute:: pyuaf.util.EndpointDescription.securityPolicyUri
        
            The security policy URI (e.g. :attr:`pyuaf.util.securitypolicies.UA_Basic128`),
            (an ``int``, as defined in :mod:`pyuaf.util.securitypolicies`).
    
        .. autoattribute:: pyuaf.util.EndpointDescription.userIdentityTokens
        
             The allowed user identity token policies (as a :class:`pyuaf.util.UserTokenPolicyVector`).
    
        .. autoattribute:: pyuaf.util.EndpointDescription.transportProfileUri
        
             The transport profile URI (type :class:`~pyuaf.util.StringVector`).
    
        .. autoattribute:: pyuaf.util.EndpointDescription.securityLevel
        
             The security level (type ``int``).
    
    
    


*class* EndpointDescriptionVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.EndpointDescriptionVector

    An EndpointDescriptionVector is a container that holds elements of type 
    :class:`pyuaf.util.EndpointDescription`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`pyuaf.util.EndpointDescription`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import EndpointDescriptionVector, EndpointDescription
        
        >>> # construct a EndpointDescriptionVector without elements:
        >>> descriptions = EndpointDescriptionVector()
        
        >>> noOfElements = len(descriptions) # will be 0
        
        >>> descriptions.append(EndpointDescription())
        
        >>> noOfElements = len(descriptions) # will be 1
        
        >>> descriptions.resize(4)
        
        >>> noOfElements = len(descriptions) # will be 4
        
        >>> description0 = descriptions[0]
        
        >>> # you may construct an EndpointDescriptionVector from a regular Python list:
        >>> otherDescriptions = EndpointDescriptionVector( [EndpointDescription(), EndpointDescription()] )
        
    



*class* EnumValue
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.EnumValue

    An EnumValue represents a child of an enumerated value definition.


    * Methods:

        .. automethod:: pyuaf.util.EnumValue.__init__
    
            Construct a new EnumValue, in one of the following ways:
        
            .. doctest::
    
                >>> import pyuaf
                >>> from pyuaf.util import EnumValue, LocalizedText
                >>> e0 = EnumValue()
                >>> e1 = EnumValue(1)
                >>> e2 = EnumValue(1, "manual")
                >>> e3 = EnumValue(1, "manual", LocalizedText("en", "Manual operation mode") )
    

        .. automethod:: pyuaf.util.EnumValue.value
    
            Get the numeric value of the EnumValue.
            
            :return: The numeric value.
            :rtype: ``int``

        .. automethod:: pyuaf.util.EnumValue.setValue
    
            Set the numeric value of the EnumValue.
            
            :param i: The new numeric value.
            :type i: ``int``
      

        .. automethod:: pyuaf.util.EnumValue.name
    
            Get the name of the EnumValue.
            
            :return: The name.
            :rtype: ``str``

        .. automethod:: pyuaf.util.EnumValue.setName
    
            Set the name of the EnumValue.
            
            :param name: The new name.
            :type name: ``str``    
      
        .. automethod:: pyuaf.util.EnumValue.documentation
    
            Get the documentation of the EnumValue.
            
            :return: Documentation about the value.
            :rtype: :class:`~pyuaf.util.LocalizedText`

        .. automethod:: pyuaf.util.EnumValue.setDocumentation
    
            Set the documentation of the EnumValue.
            
            :param documentation: The new documentation.
            :type documentation: :class:`~pyuaf.util.LocalizedText`    



*class* EventFilter
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.EventFilter

    An EventFilter selects and filters the events to be received by a client.
    
    
    * Methods:

        .. automethod:: pyuaf.util.EventFilter.__init__
            
            Create a default EventFilter instance.

        .. automethod:: pyuaf.util.EventFilter.__str__
            
            Get a string representation.

    * Attributes:
    
        .. autoattribute:: pyuaf.util.EventFilter.selectClauses
        
            The select clauses, as a :class:`~pyuaf.util.SimpleAttributeOperandVector`.




*class* ExpandedNodeId
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ExpandedNodeId


    An ExpandedNodeId fully identifies the node by means of a NodeId part, and a part containing
    the serverIndex and/or serverUri of the server hosting the node.


    * Methods:

        .. automethod:: pyuaf.util.ExpandedNodeId.__init__
    
            Construct a new ExpandedNodeId in one of the following ways:
            
            .. doctest::
            
                >>> import pyuaf
                >>> from pyuaf.util import ExpandedNodeId, NodeId
        
                >>> # suppose we have a node with the following properties:
                >>> nodeIdStringIdentifier = "myStringIdentifier"
                >>> nameSpaceUri           = "myNameSpaceUri"
                >>> nameSpaceIndex         = 3
                >>> serverUri              = "myServerUri"
                >>> serverIndex            = 2
        
                >>> # and one with the same namespace and server properties, but with a different identifier:
                >>> nodeIdNumericIdentifier = 1234
                
                >>> # then these ExpandedNodeIds all point to the nodes:
                >>> exp0  = ExpandedNodeId(nodeIdStringIdentifier  , nameSpaceUri   , serverUri   )
                >>> exp1  = ExpandedNodeId(nodeIdStringIdentifier  , nameSpaceUri   , serverIndex )
                >>> exp2  = ExpandedNodeId(nodeIdStringIdentifier  , nameSpaceUri   , serverUri   , serverIndex)
                >>> exp3  = ExpandedNodeId(nodeIdStringIdentifier  , nameSpaceIndex , serverUri   )
                >>> exp4  = ExpandedNodeId(nodeIdStringIdentifier  , nameSpaceIndex , serverIndex )
                >>> exp5  = ExpandedNodeId(nodeIdStringIdentifier  , nameSpaceIndex , serverUri   , serverIndex)
                >>> exp6  = ExpandedNodeId(nodeIdNumericIdentifier , nameSpaceUri   , serverUri   )
                >>> exp7  = ExpandedNodeId(nodeIdNumericIdentifier , nameSpaceUri   , serverIndex )
                >>> exp8  = ExpandedNodeId(nodeIdNumericIdentifier , nameSpaceUri   , serverUri   , serverIndex)
                >>> exp9  = ExpandedNodeId(nodeIdNumericIdentifier , nameSpaceIndex , serverUri   )
                >>> exp10 = ExpandedNodeId(nodeIdNumericIdentifier , nameSpaceIndex , serverIndex )
                >>> exp11 = ExpandedNodeId(nodeIdNumericIdentifier , nameSpaceIndex , serverUri   , serverIndex)
                
                >>> # you can also define the NodeId first and provide this + serverIndex and/or serverUri 
                >>> nodeId = NodeId(nodeIdStringIdentifier, nameSpaceUri, nameSpaceIndex)
                >>> exp12 = ExpandedNodeId(nodeId, serverUri   )
                >>> exp13 = ExpandedNodeId(nodeId, serverIndex )
                >>> exp14 = ExpandedNodeId(nodeId, serverUri   , serverIndex)

    
        .. automethod:: pyuaf.util.ExpandedNodeId.hasServerIndex
        
            Check if a server index was given.
            
            :return: True if a serverIndex has been defined, False if not.
            :rtype:  ``bool``

    
        .. automethod:: pyuaf.util.ExpandedNodeId.hasServerUri
        
            Check if a non-empty server URI was given.
            
            :return: True if this ExpandedNodeId contains a non-empty server URI, False if not.
            :rtype:  ``bool``

    
        .. automethod:: pyuaf.util.ExpandedNodeId.serverIndex
        
            Get the server index of the ExpandedNodeId.
          
            This function will always return an ``int``, so call hasServerIndex() to see if it's a
            valid one.
            
            :return: The server index.
            :rtype:  ``int``

    
        .. automethod:: pyuaf.util.ExpandedNodeId.serverUri
        
            Get the server URI of the ExpandedNodeId.
          
            This function will always return a string, so call hasServerUri() to see if it's a
            valid one.
            
            :return: The server URI.
            :rtype:  ``str``

    
        .. automethod:: pyuaf.util.ExpandedNodeId.nodeId()
        
            Get the NodeId part of the ExpandedNodeId.
            
            :return: The NodeId part.
            :rtype:  :class:`pyuaf.util.NodeId`



*class* ExpandedNodeIdVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ExpandedNodeIdVector

    A ExpandedNodeIdVector is a container that holds elements of type 
    :class:`pyuaf.util.ExpandedNodeId`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`~pyuaf.util.ExpandedNodeId`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import ExpandedNodeIdVector, ExpandedNodeId
        
        >>> # construct a ExpandedNodeIdVector without elements:
        >>> vec = ExpandedNodeIdVector()
        
        >>> noOfElements = len(vec) # will be 0
        
        >>> vec.append(ExpandedNodeId("some id", "http://Some/Namespace/URI", "Some:Server:URI"))
        
        >>> noOfElements = len(vec) # will be 1
        
        >>> if vec[0].hasServerUri():
        ...    serverUri = vec[0].serverUri()
        ... else:
        ...    serverUri = None
        
        >>> vec.resize(2)
        
        >>> noOfElements = len(vec) # will be 2
        
        >>> # you may construct a ExpandedNodeIdVector from a regular Python list:
        >>> someOtherVec = ExpandedNodeIdVector( [ ExpandedNodeId("id 0", "MyNamespace", "serverX"), 
        ...                                        ExpandedNodeId("id 1", "MyNamespace", "serverY") ] )


    



*class* ExtensionObject
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ExtensionObject

    An ExtensionObject is a non-standard kind of data that can be encoded/decoded and be sent over 
    the wire.
    
    * Methods:

        .. automethod:: pyuaf.util.ExtensionObject.__init__
    
            Construct a new ExtensionObject.
    

    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.ExtensionObject.encodingTypeId
        
            The NodeId that describes the encoding (type :class:`~pyuaf.util.NodeId`).
    
        .. autoattribute:: pyuaf.util.ExtensionObject.dataTypeId
        
            The NodeId that describes the datatype (type :class:`~pyuaf.util.NodeId`).
    

*class* Guid
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.Guid

    A Guid represents a Globally unique identifier.
    
    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import Guid, NodeId, Address
        >>>
        >>> # create a NodeId for a known node:
        >>> namespaceUri = "http://www.vendorXYZ.com/products/temperaturecontrollers"
        >>> guid         = Guid("{00000000-0000-0000-0000-000000000001}")
        >>> nodeId       = NodeId(guid, namespaceUri)
        >>>
        >>> # create an address which points to that NodeId hosted by a specific server:
        >>> serverUri = "/ourcompany/plantfloor/temperaturecontrollers/3"
        >>> address   = Address(nodeId, serverUri)
        >>>
        >>> # now we can create a Client and read/write/monitor/... the addressed node
    
    * Methods:

        .. automethod:: pyuaf.util.Guid.__init__
            
            Create a Guid instance, without arguments or with a UTF-8 ``str`` argument (e.g. 
            "{00000000-0000-0000-0000-000000000001}").
        
        .. automethod:: pyuaf.util.Guid.__str__
            
            Get a UTF-8 string representation.

        .. automethod:: pyuaf.util.Guid.fromString
            
            Change the value of the GUID by providing a UTF-8 string (of type ``str``).




*class* LocalizedText
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.LocalizedText


    A LocalizedText is text with a locale attached.


    * Methods:

        .. automethod:: pyuaf.util.LocalizedText.__init__([locale[, text]])
    
            Construct a new LocalizedText in one of the following ways:
            
            .. doctest::
            
                >>> import pyuaf
                >>> from pyuaf.util import LocalizedText
                
                >>> # the following ways of constructing a QualifiedName are possible:
                >>> lText0 = LocalizedText()                        # incomplete, no information given!
                >>> lText1 = LocalizedText("en", "My English Text") # OK
                >>> lText2 = LocalizedText("", "My Text")           # OK (no specific locale given)
                
                >>> # strings are assumed to be UTF-8 encoded!
                >>> unicodeObject = u"40\u00B0 Celsius ist eine hei\u00DFe Temperatur!"
                >>> lText3 = LocalizedText("de", unicodeObject.encode("UTF-8"))
    
    
        .. automethod:: pyuaf.util.LocalizedText.isNull
        
            Check if the localized text has a NULL value.
            When the localized text has a null value, the :attr:`~pyuaf.util.LocalizedText.text` and
            :attr:`~pyuaf.util.LocalizedText.locale` methods will return empty strings.
            
            :return: True if NULL.
            :rtype:  ``bool``

    
        .. automethod:: pyuaf.util.LocalizedText.text
        
            Get the text part of the LocalizedText, as an UTF-8 encoded string.
            
            :return: The text part.
            :rtype:  ``str``
    
    
        .. automethod:: pyuaf.util.LocalizedText.locale
        
            Get the locale part of the LocalizedText.
            
            :return: The locale part (e.g. "en", "de", "nl", "", ...).
            :rtype:  ``str``
    
    
        .. automethod:: pyuaf.util.LocalizedText.toFullString
        
            Get a string of the locale part + the text part.
            
            :return: The localized text.
            :rtype:  ``str``




*class* LoggingInterface
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.LoggingInterface

    A LoggingInterface interface class can be implemented to receive log messages.
    
    For instance, the pyuaf.client.Client class is implementing this interface.
    As a regular UAF user, you should not have to deal with this interface directly.
    
    * Methods:

        .. automethod:: pyuaf.util.LoggingInterface.__dispatch_logMessageReceived__(message)

            A new log message was received.

            :param message: The log message that was received.
            :type  message: :class:`pyuaf.util.LogMessage`



*class* LogMessage
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.LogMessage

    A LogMessage instance contains a timestamp, a log level, an informative message, etc.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> import time
        
        >>> # normally you receive log messages from a pyuaf.client.Client instance,
        >>> # but in this case we'll create a log message manually:
        >>> msg = pyuaf.util.LogMessage(pyuaf.util.loglevels.Debug, 
        ...                             "MyApp", 
        ...                             "MyLogger", 
        ...                             "Something has happened,\n" 
        ...                             + "requiring a big multi-line description!")
        
        >>> niceLocalTimeString =  time.strftime("%a, %d %b %Y %H:%M:%S", time.localtime(msg.ctime))
        >>> niceLocalTimeString += ".%.3d" %msg.msec
        
        >>> stringToWrittenToFile  = "New log message:"
        >>> stringToWrittenToFile += " - time received    : %s" %niceLocalTimeString
        >>> stringToWrittenToFile += " - application name : %s" %msg.applicationName
        >>> stringToWrittenToFile += " - logger name      : %s" %msg.loggerName
        >>> stringToWrittenToFile += " - level            : %d = %s" %(msg.level, pyuaf.util.loglevels.toString(msg.level))
        >>> for line in msg.message.splitlines():
        ...     stringToWrittenToFile += " - message lines    : %s" %line
        
        >>> theWholeMessageAtOnce = str(msg)
        

    * Methods:

        .. automethod:: pyuaf.util.LogMessage.__init__(level, applicationName, loggerName, message)
    
            Construct a new LogMessage.
            
            The 'ctime' timestamp will be set to the construction time.
            
    
        .. automethod:: pyuaf.util.LogMessage.__str__
    
            Get a formatted string representation of the log message.


    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.LogMessage.ctime
        
            The timestamp as a 64-bit number (the "c-time") expressing the seconds that passed
            since the epoch, as an ``int``.
  
        .. autoattribute:: pyuaf.util.LogMessage.msec
        
            The number of milliseconds past the ctime, as an ``int``.
  
        .. autoattribute:: pyuaf.util.LogMessage.level
        
            The log level as an ``int`` (as defined in :mod:`pyuaf.util.loglevels`).
    
        .. autoattribute:: pyuaf.util.LogMessage.applicationName
        
            The name of the application that owns the logger (type ``str``).
    
        .. autoattribute:: pyuaf.util.LogMessage.loggerName
        
            The name of the logger that produced the log message (type ``str``).
    
        .. autoattribute:: pyuaf.util.LogMessage.message
        
            The actual informative message to be logged (type ``str``).
    
    



*class* Mask
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.Mask


    This is a boolean mask to mark items as 'set' (boolean True) or 'unset' (boolean False).
    
    The mask keeps track of the number of 'set' and 'unset' values. This means you can
    very efficiently call countSet() and countUnset().


    * Methods:

        .. automethod:: pyuaf.util.Mask.__init__([initialSize[, initialValue]])
    
            Construct a new Mask.
            
            The initial size can optionally be given. 
            If the initial size is given, also the initial value of all initial elements can be 
            given.
            
            :param initialSize: (Optional) Initial number of elements of the mask.
            :type  initialSize: ``int``
            :param initialValue: (Optional) Value (True or False) of the initial elements.
            :type  initialValue: ``bool``
                
                
    
        .. automethod:: pyuaf.util.Mask.resize(newSize)
        
            Resize the mask.
            
            :param newSize: The size the mask should be having.
            :type  newSize: ``int``
    
    
        .. automethod:: pyuaf.util.Mask.size
        
            The current size of the mask.
            
            :return: The current number of elements.
            :rtype:  ``int``
    
    
        .. automethod:: pyuaf.util.Mask.setCount
        
            Get the number of 'set' elements.
            
            :return: The current number of 'set' elements.
            :rtype:  ``int``
    
    
        .. automethod:: pyuaf.util.Mask.unsetCount
        
            Get the number of 'unset' elements.
            
            :return: The current number of 'unset' elements.
            :rtype:  ``int``
    
    
        .. automethod:: pyuaf.util.Mask.set(i)
        
            Set the i'th mask item (i.e. make it True).
            
            :param i: The number of the item that should be 'set'.
            :type  i: ``int``
    
    
        .. automethod:: pyuaf.util.Mask.unset(i)
        
            Unset the i'th mask item (i.e. make it False).
            
            :param i: The number of the item that should be 'unset'.
            :type  i: ``int``
    
    
        .. automethod:: pyuaf.util.Mask.__and__(otherMask)
        
            Logically AND this mask with another mask element-wise.
            
            :param otherMask: The other mask.
            :type  otherMask: :class:`pyuaf.util.Mask`
            :return: A new mask, the logical AND result of the current one and the other one.
            :rtype:  :class:`pyuaf.util.Mask`




*class* ModelChangeStructureDataType
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ModelChangeStructureDataType

    A ModelChangeStructureDataType is usually passed as an event to notify model changes.
    
    * Methods:

        .. automethod:: pyuaf.util.ModelChangeStructureDataType.__init__
    
            Construct a new ModelChangeStructureDataType.
    

    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.ModelChangeStructureDataType.affected
        
            The NodeId of the affected node (type :class:`~pyuaf.util.NodeId`).
    
        .. autoattribute:: pyuaf.util.ModelChangeStructureDataType.affectedType
        
            The NodeId of the affected datatype (type :class:`~pyuaf.util.NodeId`).
    
        .. autoattribute:: pyuaf.util.ModelChangeStructureDataType.affectedType
        
            The verb field (an ``int``).
    


*class* ModificationInfo
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ModificationInfo

    A ModificationInfo instance holds some information about a historical data modification.
    
    
    * Methods:

        .. automethod:: pyuaf.util.ModificationInfo.__init__
            
            Create a default ModificationInfo instance.

        .. automethod:: pyuaf.util.ModificationInfo.__str__
            
            Get a string representation.


    * Class attributes:

        * Possible HistoryUpdateType values:

            .. autoattribute:: pyuaf.util.ModificationInfo.HistoryUpdateType_Insert
    
                An ``int`` identifying the type of modification: in this case an insertion!
    
            .. autoattribute:: pyuaf.util.ModificationInfo.HistoryUpdateType_Replace
    
                An ``int`` identifying the type of modification: in this case a replacement!
    
            .. autoattribute:: pyuaf.util.ModificationInfo.HistoryUpdateType_Update
    
                An ``int`` identifying the type of modification: in this case an update! 
                (in percent).
                
            .. autoattribute:: pyuaf.util.ModificationInfo.HistoryUpdateType_Delete
    
                An ``int`` identifying the type of modification: in this case a deletion!

    * Attributes:
    
        .. autoattribute:: pyuaf.util.ModificationInfo.modificationTime
        
            The time when the modification took place, as a :class:`~pyuaf.util.DateTime`.
    
        .. autoattribute:: pyuaf.util.ModificationInfo.historyUpdateType
        
            The type of the modification of the historical data, as an ``int``. This value
            should be one of the following values:
            
             - :attr:`pyuaf.util.ModificationInfo.HistoryUpdateType_Insert`
             - :attr:`pyuaf.util.ModificationInfo.HistoryUpdateType_Replace`
             - :attr:`pyuaf.util.ModificationInfo.HistoryUpdateType_Update`
             - :attr:`pyuaf.util.ModificationInfo.HistoryUpdateType_Delete`

        .. autoattribute:: pyuaf.util.ModificationInfo.userName
        
            Name of the user that modified the data, as a ``str``.



*class* ModificationInfoVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ModificationInfoVector

    A ModificationInfoVector is a container that holds elements of type 
    :class:`pyuaf.util.ModificationInfo`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`~pyuaf.util.ModificationInfo`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import ModificationInfoVector, ModificationInfo, DateTime
        
        >>> # construct a ModificationInfoVector without elements:
        >>> vec = ModificationInfoVector()
        
        >>> noOfElements = len(vec) # will be 0
        
        >>> vec.append(ModificationInfo())
        
        >>> noOfElements = len(vec) # will be 1
        
        >>> vec[0].userName = "John"
        
        >>> vec.resize(2)
        >>> noOfElements = len(vec) # will be 2
        >>> vec[1].userName          = "Wim"
        >>> vec[1].historyUpdateType = ModificationInfo.HistoryUpdateType_Delete
        >>> vec[1].modificationTime  = DateTime.now()
        
        >>> # you may construct a ModificationInfoVector from a regular Python list:
        >>> someOtherVec = ModificationInfoVector( [ ModificationInfo(), ModificationInfo() ] )


    



*class* NodeId
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.NodeId


    A NodeId identifies the node within an address space. It is not bound to a specific server
    (use an :class:`pyuaf.util.ExpandedNodeId` for that).


    * Methods:

        .. automethod:: pyuaf.util.NodeId.__init__
    
            Construct a NodeId.
            
            A NodeId should contain an identifier (e.g. a string or a numeric value) and 
            some information about the namespace (a namespace URI and/or a namespace index).
            
            Usage example:
            
            .. doctest::
            
                >>> import pyuaf
                >>> from pyuaf.util import NodeId, Guid, NodeIdIdentifier
                >>>
                >>> nsUri = "myNameSpaceUri"
                >>> nsIndex = 3
                >>> nodeId0 = NodeId() # contains no information
                >>> nodeId1 = NodeId("myStringIdentifier", nsUri)
                >>> nodeId2 = NodeId("myStringIdentifier", nsUri, nsIndex)  # nsIndex is redundant information, avoid!
                >>> nodeId3 = NodeId("myStringIdentifier", nsIndex)         # namespace indexes may change, better use namespace uris!
                >>> nodeId4 = NodeId(2345, nsUri)    
                >>> nodeId5 = NodeId(2345, nsUri, nsIndex)   # nsIndex is redundant information, avoid!
                >>> nodeId6 = NodeId(2345, nsIndex)          # namespace indexes may change, better use namespace uris!
                >>> guid = Guid("{00000000-0000-0000-0000-000000000001}")
                >>> nodeId7 = NodeId(guid, nsUri)
                >>> nodeId8 = NodeId(guid, nsUri, nsIndex) # nsIndex is redundant information here, avoid!
                >>> nodeId9 = NodeId(guid, nsIndex)        # namespace indexes may change, better use namespace uris!
                >>> identifier = NodeIdIdentifier("myStringIdentifier")
                >>> nodeId10 = NodeId(identifier, nsUri)
                >>> nodeId11 = NodeId(identifier, nsUri, nsIndex) # nsIndex is redundant information here, avoid!
                >>> nodeId12 = NodeId(identifier, nsIndex)        # namespace indexes may change, better use namespace uris!
                

    
        .. automethod:: pyuaf.util.NodeId.hasNameSpaceUri
        
            Check if a non-empty namespace URI was given.
            
            :return: True if this NodeId contains a non-empty server URI, False if not.
            :rtype:  ``bool``
    
    
        .. automethod:: pyuaf.util.NodeId.hasNameSpaceIndex
        
            Check if a namespace index was given.
            
            :return: True if a namespaceIndex has been defined, False if not.
            :rtype:  ``bool``

    

        .. automethod:: pyuaf.util.NodeId.nameSpaceUri
        
            Get the namespace URI of the NodeId.
          
            This function will always return a string, so call hasNameSpaceUri() to see if it's a
            valid one.
            
            :return: The namespace URI.
            :rtype:  ``str``

    
        .. automethod:: pyuaf.util.NodeId.nameSpaceIndex
        
            Get the namespace index of the NodeId.
          
            This function will always return an ``int``, so call hasNameSpaceIndex() to see if it's
            a valid one.
            
            :return: The namespace index.
            :rtype:  ``int``

    
        .. automethod:: pyuaf.util.NodeId.identifier
        
            Get the NodeIdIdentifier part of the NodeId.
            
            :return: The NodeIdIdentifier part.
            :rtype:  :class:`pyuaf.util.NodeIdIdentifier`


    
        .. automethod:: pyuaf.util.NodeId.setNameSpaceUri(uri)
        
            Set the namespace URI of the NodeId.
            
            :param uri: The namespace URI.
            :type  uri:  ``str``

    
        .. automethod:: pyuaf.util.NodeId.setNameSpaceIndex(index)
        
            Set the namespace index of the NodeId.
            
            :param uri: The namespace index.
            :type  uri:  ``int``


        .. automethod:: pyuaf.util.NodeId.isNull
        
            Was the NodeId initialized with some contents?
            
            :return: True if the NodeId does not have any contents.
            :rtype:  ``bool``




*class* NodeIdIdentifier
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.NodeIdIdentifier


    A NodeIdIdentifier is the identifier part of a NodeId.
    

    * Methods:

        .. automethod:: pyuaf.util.NodeIdIdentifier.__init__
    
            Construct a NodeIdIdentifier.
            
            The constructor accepts
             - an ``int`` in order to construct a Numeric identifier
             - a ``str`` in order to construct a String identifier
             - a :class:`~pyuaf.util.Guid` in order to construct a Guid identifier
             - a ``bytearray`` in order to construct a Guid identifier

        .. automethod:: pyuaf.util.NodeIdIdentifier.isNull
        
            Was the identifier initialized with some contents?
            
            :return: True if the identifier does not have any contents.
            :rtype:  ``bool``


    * Attributes:
    
        .. autoattribute:: pyuaf.util.NodeIdIdentifier.type
        
            The type of the NodeIdIdentifier (e.g. :attr:`pyuaf.util.nodeididentifiertypes.Identifier_String`)
            as defined within :mod:`pyuaf.util.nodeididentifiertypes`.
    
        .. autoattribute:: pyuaf.util.NodeIdIdentifier.idString
        
            The ``str`` value in case :attr:`~pyuaf.util.NodeIdIdentifier.type` equals 
            :attr:`pyuaf.util.nodeididentifiertypes.Identifier_String`.
    
        .. autoattribute:: pyuaf.util.NodeIdIdentifier.idNumeric
        
            The ``int`` value in case :attr:`~pyuaf.util.NodeIdIdentifier.type` equals 
            :attr:`pyuaf.util.nodeididentifiertypes.Identifier_Numeric`.
    
        .. autoattribute:: pyuaf.util.NodeIdIdentifier.idGuid
        
            The :class:`~pyuaf.util.Guid` value in case :attr:`~pyuaf.util.NodeIdIdentifier.type` equals 
            :attr:`pyuaf.util.nodeididentifiertypes.Identifier_Guid`.
    
        .. autoattribute:: pyuaf.util.NodeIdIdentifier.idOpaque
        
            The ``bytearray`` (a built-in Python type) value in case 
            :attr:`~pyuaf.util.NodeIdIdentifier.type` equals 
            :attr:`pyuaf.util.nodeididentifiertypes.Identifier_Opaque`.


*class* PkiCertificate
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.PkiCertificate

    A PkiCertificate holds a X509 certificate.

    Here's some example code to set up a certificate:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import PkiRsaKeyPair, PkiIdentity, PkiCertificateInfo, PkiCertificate
        
        >>> # we'll need the hostname of the computer later on:
        >>> import socket
        >>> hostName = socket.gethostname()
        
        >>> # we will create a self-signed certificate, which means that the subject and issuer are the same!
        
        >>> keyPair = PkiRsaKeyPair(1024)
        >>> issuerPrivateKey = keyPair.privateKey()
        >>> subjectPublicKey = keyPair.publicKey()
        
        >>> identity = PkiIdentity()
        >>> identity.commonName         = "Wim Pessemier"
        >>> identity.organization       = "KU Leuven"
        >>> identity.organizationUnit   = "Institute of Astronomy"
        >>> identity.locality           = "Leuven"
        >>> identity.country            = "BE"
        
        >>> info = PkiCertificateInfo()
        >>> info.uri        = "urn:%s:InstituteOfAstronomy::MyExampleCode" %hostName # must be unique!
        >>> info.dns        = hostName
        >>> info.eMail      = "Wxx.Pxxxxxxxx@ster.kuleuven.be"
        >>> info.validTime  = 60*60*24*365*5 # 5 years
        
        >>> certificate = PkiCertificate(info, identity, subjectPublicKey,  identity, issuerPrivateKey)

        
    * Class attributes:
    
        .. autoattribute:: pyuaf.util.PkiCertificate.Action_Reject
        .. autoattribute:: pyuaf.util.PkiCertificate.Action_AcceptTemporarily
        .. autoattribute:: pyuaf.util.PkiCertificate.Action_AcceptPermanently
        
        .. autoattribute:: pyuaf.util.PkiCertificate.Extension_SubjectAltName
        .. autoattribute:: pyuaf.util.PkiCertificate.Extension_BasicConstraints
        .. autoattribute:: pyuaf.util.PkiCertificate.Extension_NetscapeComment
        .. autoattribute:: pyuaf.util.PkiCertificate.Extension_SubjectKeyIdentifier
        .. autoattribute:: pyuaf.util.PkiCertificate.Extension_AuthorityKeyIdentifier
        .. autoattribute:: pyuaf.util.PkiCertificate.Extension_KeyUsage
        .. autoattribute:: pyuaf.util.PkiCertificate.Extension_ExtendedKeyUsage
        
        
    * Methods:

        .. automethod:: pyuaf.util.PkiCertificate.__init__(*args)
        
            Construct a new PkiCertificate with the following arguments:
            
            :param info:                The certificate info.
            :type  info:                :class:`~pyuaf.util.PkiCertificateInfo`
            :param subject:             The subject identity.
            :type  subject:             :class:`~pyuaf.util.PkiIdentity`
            :param subjectPublicKey:    The subject public key.
            :type  subjectPublicKey:    :class:`~pyuaf.util.PkiPublicKey`
            :param issuer:              The issuer identity.
            :type  issuer:              :class:`~pyuaf.util.PkiIdentity`
            :param issuerPrivateKey:    The issuer private key.
            :type  issuerPrivateKey:    :class:`~pyuaf.util.PkiPrivateKey`
            
        .. automethod:: pyuaf.util.PkiCertificate.isNull
            
            Check if the certificate has data.
            
            :return: True if the certificate has data, False if not.
            :rtype:  ``bool``
            
        .. automethod:: pyuaf.util.PkiCertificate.isValid
            
            Check if the signature is not expired.
            
            :return: True if the certificate is valid, False if not.
            :rtype:  ``bool``
            
        .. automethod:: pyuaf.util.PkiCertificate.isSelfSigned
            
            Check if the certificate is self-signed (i.e. if the subject and issuer are the same).
            
            :return: True if the certificate is self-signed, False if not.
            :rtype:  ``bool``

        .. automethod:: pyuaf.util.PkiCertificate.publicKey
        
            Get the public key of the certificate.
            
            :return: The public key of the certificate.
            :rtype:  :class:`~pyuaf.util.PkiPublicKey`

        .. automethod:: pyuaf.util.PkiCertificate.commonName
        
            Convenience method to get the common name of the subject.
            (equals ``myPkiCertificate.subject().commonName``).
            
            :return: The common name of the subject.
            :rtype: ``str``
            
        .. automethod:: pyuaf.util.PkiCertificate.subject
            
            Get the identity of the subject of the certificate.
            
            :return: The subject.
            :rtype:  :class:`~pyuaf.util.PkiIdentity`
            
        .. automethod:: pyuaf.util.PkiCertificate.issuer
            
            Get the identity of the issuer of the certificate.
            
            :return: The issuer.
            :rtype:  :class:`~pyuaf.util.PkiIdentity`
            
        .. automethod:: pyuaf.util.PkiCertificate.subjectNameHash
            
            Get the hash of the subject name.
            
            :return: The hash of the subject name (e.g. 1877523877).
            :rtype:  ``long``
            
        .. automethod:: pyuaf.util.PkiCertificate.info
            
            Get information from the X509v3 extension "subjectAltName" (SAN). It allows various
            values to be associated with a security certificate.
            
            .. warning:: 
                    
                    :attr:`~pyuaf.util.PkiCertificateInfo.validTime` will not be filled, 
                    use :meth:`~pyuaf.util.PkiCertificate.validFrom` and 
                    :meth:`~pyuaf.util.PkiCertificate.validTo` instead!
            
            :return: The certificate info **without** validTime.
            :rtype:  :class:`~pyuaf.util.PkiCertificateInfo`
            
        .. automethod:: pyuaf.util.PkiCertificate.validFrom
            
            Get the start date from when the certificate is valid.
            
            :return: The start date from when the certificate is valid.
            :rtype:  :class:`~pyuaf.util.DateTime`
            
        .. automethod:: pyuaf.util.PkiCertificate.validTo
            
            Get the end date until when the certificate is valid. 
            
            :return: The end date until when the certificate is valid. 
            :rtype:  :class:`~pyuaf.util.DateTime`
            
        .. automethod:: pyuaf.util.PkiCertificate.serialNumber
            
            Get the X.509 serial number (a unique number issued by the certificate issuer),
            as a hexadecimal string.
            
            :return: The serial number (e.g. "542BA97B").
            :rtype:  ``str``
            
        .. automethod:: pyuaf.util.PkiCertificate.signatureTypeNID
            
            Get the numerical ID (NID) of the signature algorithm type.
            
            :return: The signature algorithm type NID (e.g. 65).
            :rtype:  ``int``
            
        .. automethod:: pyuaf.util.PkiCertificate.signatureTypeString
            
            Get a string representation of the numerical ID (NID) of the signature algorithm type.
            
            :return: The signature algorithm type as a string (e.g. "RSA-SHA1").
            :rtype:  ``str``
            
        .. automethod:: pyuaf.util.PkiCertificate.hasExtension
            
            Check if the certificate has the given extension. 
            
            :param extension: Extension ID, e.g. :attr:`pyuaf.util.PkiCertificate.Extension_SubjectAltName`.
            :type extension: ``int``
            :return: True if the extension is present, False if not.
            :rtype:  ``bool``
            
        .. automethod:: pyuaf.util.PkiCertificate.extensionValue
            
            Get the value of the given extension if the extension is present.
            
            :param extension: Extension ID, e.g. :attr:`pyuaf.util.PkiCertificate.Extension_SubjectAltName`.
            :type extension: ``int``
            :return: The value as a string.
            :rtype:  ``str``
            
            
        .. automethod:: pyuaf.util.PkiCertificate.toDER
            
            Get a DER encoded bytearray of the certificate.
            
            :return: DER encoded data.
            :rtype:  ``bytearray``
            
        .. automethod:: pyuaf.util.PkiCertificate.toDERFile
            
            Write the certificate to a DER-encoded file with the given filename.
            An error code is returned (0 if success).
            
            Note that the directories in which the file should reside, must already exist! This
            function will only create a file, no directories!
            
            :param fileName: The filename (may be relative or absolute).
            :type fileName: ``str``
            :return: Error code (0 if success).
            :rtype:  ``int``
            
        .. automethod:: pyuaf.util.PkiCertificate.toPEMFile
            
            Write the certificate to a PEM-encoded file with the given filename.
            An error code is returned (0 if success).
            
            Note that the directories in which the file should reside, must already exist! This
            function will only create a file, no directories!
            
            :param fileName: The filename (may be relative or absolute).
            :type fileName: ``str``
            :return: Error code (0 if success).
            :rtype:  ``int``
            
        .. automethod:: pyuaf.util.PkiCertificate.thumbPrint
            
            Get a SHA1 thumb print (finger print) of the certficate. It is a short sequence of
            bytes used to identify a certificate efficiently.
            
            :return: Thumb print data.
            :rtype:  ``bytearray``
            
        .. automethod:: pyuaf.util.PkiPublicKey.getErrors
        
            Get a list of errors.
            
            :return: A list of error descriptions.
            :rtype:  ``list`` of ``str``
            
        
    * Class methods:

        .. automethod:: pyuaf.util.PkiCertificate.fromDER
        
            Static method to get a certificate from a DER encoded bytearray.
            
            :param data: DER data.
            :type data: ``bytearray``
            :return: A new certificate instance.
            :rtype: :class:`~pyuaf.util.PkiCertificate`

        .. automethod:: pyuaf.util.PkiCertificate.fromDERFile
        
            Static method to get a certificate from a DER encoded file.
            
            :param fileName: File name of the DER file.
            :type fileName: ``str``
            :return: A new certificate instance.
            :rtype: :class:`~pyuaf.util.PkiCertificate`

        .. automethod:: pyuaf.util.PkiCertificate.fromPEMFile
        
            Static method to get a certificate from a PEM encoded file.
            
            :param fileName: File name of the PEM file.
            :type fileName: ``str``
            :return: A new certificate instance.
            :rtype: :class:`~pyuaf.util.PkiCertificate`
            
            


*class* PkiCertificateInfo
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.PkiCertificateInfo

     A PkiCertificateInfo holds some information about a PkiCertificate.


    * Methods:

        .. automethod:: pyuaf.util.PkiCertificateInfo.__init__
    
            Construct a new PkiCertificateInfo.
    
    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.PkiCertificateInfo.uri
        
            The URI of the application certificate, as a string (type ``str``).
  
        .. autoattribute:: pyuaf.util.PkiCertificateInfo.ipAddresses
        
            The IP addresses of the application certificate, as a :class:`~pyuaf.util.StringVector`.
            Optional if no DNS is available.
            
        .. autoattribute:: pyuaf.util.PkiCertificateInfo.dnsNames
        
            The DNS names of the application certificate, as a :class:`~pyuaf.util.StringVector`.
            
        .. autoattribute:: pyuaf.util.PkiCertificateInfo.eMail
        
            The e-mail address of the application certificate, as a string (type ``str``).
            
        .. autoattribute:: pyuaf.util.PkiCertificateInfo.validTime
        
            The valid time of the application certificate, in seconds, as a ``long``.
            
            
         

*class* PkiPrivateKey
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.PkiPrivateKey

     A PkiPrivateKey is just a container for a private key.
      
    .. note::
    
        A private key only makes sense in combination with a public key. Therefore you should never
        create a PkiPrivateKey instance using the constructor of PkiPrivateKey. Instead, always 
        create a :class:`~pyuaf.util.PkiRsaKeyPair` and get the private key from there (i.e. via its
        :meth:`~pyuaf.util.PkiRsaKeyPair.privateKey` method). See the documentation of 
        :class:`~pyuaf.util.PkiRsaKeyPair` to see example code of how to create a private key
        via a key pair.

    * Methods:

        .. automethod:: pyuaf.util.PkiPrivateKey.__init__
    
            Construct a new PkiPrivateKey... in a **bad** way! 
            **See the note above to understand why you should avoid calling this constructor 
            directly!**
       


*class* PkiPublicKey
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.PkiPublicKey

     A PkiPublicKey can hold an RSA or DSA encrypted public key.
      
    .. note::
    
        A public key only makes sense in combination with a private key. Therefore you should never
        create a PkiPublicKey instance using the constructor of PkiPublicKey. Instead, always 
        create a :class:`~pyuaf.util.PkiRsaKeyPair` and get the public key from there (i.e. via its
        :meth:`~pyuaf.util.PkiRsaKeyPair.publicKey` method). See the documentation of 
        :class:`~pyuaf.util.PkiRsaKeyPair` to see example code of how to create a public key
        via a key pair.
        
    .. warning::
       
       The SDK (up to 1.4.2) contains a bug so that your application may crash (segmentation fault)
       if the data of the PkiPublicKey make no sense. To avoid this, always create a public key from
       a key pair (which should also contain sensible data).

    * Class attributes:
  
        .. autoattribute:: pyuaf.util.PkiPublicKey.RSA
        .. autoattribute:: pyuaf.util.PkiPublicKey.DSA
        .. autoattribute:: pyuaf.util.PkiPublicKey.Unknown
  
    * Class methods:
    
        .. automethod:: pyuaf.util.PkiPublicKey.fromDER(data)
        
            Read the public key from a DER encoded ``bytearray``.
            
            :param data: The DER encoded data.
            :type  data:  ``bytearray``
            :return: A new PkiPublicKey instance.
            :rtype: :class:`~pyuaf.util.PkiPublicKey`
    

    * Instance Methods:

        .. automethod:: pyuaf.util.PkiPublicKey.__init__
    
            Construct a new PkiPublicKey... in a **bad** way! 
            **See the note above to understand why you should avoid calling this constructor 
            directly!**
            
    
    * Attributes:
            
        .. automethod:: pyuaf.util.PkiPublicKey.keyType
        
            Get the type of the key.
            
            :return: Either :attr:`~pyuaf.util.PkiPublicKey.RSA` 
                     or :attr:`~pyuaf.util.PkiPublicKey.DSA`
                     or :attr:`~pyuaf.util.PkiPublicKey.Unknown`.
            :rtype:  ``int``
            
        .. automethod:: pyuaf.util.PkiPublicKey.keySize
        
            Get the size of the key.
            
            :return: The size of the key.
            :rtype:  ``int``
  
        .. automethod:: pyuaf.util.PkiPublicKey.toDER
        
            Write the public key to a DER encoded ``bytearray``. 
            
            :return: The DER encoded bytearray.
            :rtype:  ``bytearray``
            
        .. automethod:: pyuaf.util.PkiPublicKey.getErrors
        
            Get a list of errors.
            
            :return: A list of error descriptions.
            :rtype:  ``list`` of ``str``
            
            

*class* PkiRsaKeyPair
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.PkiRsaKeyPair

    A PkiRsaKeyPair holds a private and a public key pair.
    
    Creating a PkiRsaKeyPair is the only sensible way to create a :class:`~pyuaf.util.PkiPublicKey`
    and :class:`~pyuaf.util.PkiPrivateKey`:
    
    .. doctest::
    
        >>> import pyuaf
        >>> pair = pyuaf.util.PkiRsaKeyPair(1024)
        >>> publicKey = pair.publicKey()
        >>> privateKey = pair.privateKey()
        
    .. warning::
       
       The SDK (up to 1.4.2) contains a bug so that your application may crash (segmentation fault)
       if the data of the key pair make no sense. To avoid this, always make sure your key pair
       has sensible data (e.g. don't use :meth:`pyuaf.util.PkiRsaKeyPair.fromPEMFile` with gibberish
       data).
  
    * Class methods:
    
        .. automethod:: pyuaf.util.PkiRsaKeyPair.fromPEMFile(args*)
        
            Read the key pair from a PEM file.
            
            If the file is not protected by a password, then just provide one argument 
            (the file name).
            If the file is protected by a password, then provide 2 arguments (the file name 
            must be the first argument, the password must be the second argument).
            
            :param fileName: The filename, e.g. "keypair.pem" or "somefolder/keypair.pem" or 
                             an absolute path.
            :type  fileName: ``str``
            :param password: The password of the file.
            :type  password: ``str``
            :return: A new PkiRsaKeyPair instance.
            :rtype: :class:`~pyuaf.util.PkiRsaKeyPair`
    
        .. automethod:: pyuaf.util.PkiRsaKeyPair.checkKeyPair(args*)
        
            Check if a public and private key pair matches.
            
            :param publicKey:  The public key.
            :type publicKey:   :class:`~pyuaf.util.PkiPublicKey`
            :param privateKey: The private key.
            :type privateKey:  :class:`~pyuaf.util.PkiPrivateKey`
            :return:           True if the keys match, False if not.
            :rtype:            ``bool``
            
            .. warning::
            
                This method may **crash** when empty keys are checked, due to a bug in the SDK!!! 
                Bug present in SDK v1.4.2. 
            

    * Instance Methods:

        .. automethod:: pyuaf.util.PkiRsaKeyPair.__init__(bits = 1024)
    
            Construct a new PkiRsaKeyPair.
            
            :param bits: The encryption bitsize.
            :type  bits: ``int``
        
        .. automethod:: pyuaf.util.PkiRsaKeyPair.privateKey
    
            Get the private key of this pair.
            
            :return: The private key.
            :rtype:  :class:`~pyuaf.util.PkiPrivateKey`
        
        .. automethod:: pyuaf.util.PkiRsaKeyPair.publicKey
    
            Get the public key of this pair.
            
            :return: The public key.
            :rtype:  :class:`~pyuaf.util.PkiPublicKey`
        
        .. automethod:: pyuaf.util.PkiRsaKeyPair.toPEMFile(args*)
        
            Write the key pair to an unprotected PEM file.
            
            If the file is not protected by a password, then just provide one argument 
            (the file name).
            If the file is protected by a password, then provide 2 arguments (the file name 
            must be the first argument, the password must be the second argument).
            
            Note that the directories in which the file should reside, must already exist! This
            function will only create a file, no directories!
            
            :param fileName: The filename, e.g. "keypair.pem" or "somefolder/keypair.pem" or 
                             an absolute path.
            :type  fileName: ``str``
            :param password: The password of the file.
            :type  password: ``str``
            :return: exit code (0 on success).
            :rtype: ``int``
  
        .. automethod:: pyuaf.util.PkiRsaKeyPair.toDER
        
            Write the key pair to a DER encoded ``bytearray``. 
            
            :return: The DER encoded bytearray.
            :rtype:  ``bytearray``
  
        .. automethod:: pyuaf.util.PkiRsaKeyPair.isValid
        
            Check if the key pair is valid.
            
            :return: True if the key pair is valid, False if not.
            :rtype:  ``bool``
            
        .. automethod:: pyuaf.util.PkiRsaKeyPair.getErrors
        
            Get a list of errors.
            
            :return: A list of error descriptions.
            :rtype:  ``list`` of ``str``
            
    
            

*class* PkiIdentity
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.PkiIdentity

     PkiIdentity holds the identity of the subject and issuer of a certificate.


    * Methods:

        .. automethod:: pyuaf.util.PkiIdentity.__init__
    
            Construct a new PkiIdentity.
    
        .. automethod:: pyuaf.util.PkiIdentity.isEmpty
    
            Is the PKI identity filled out, or empty?
            
            :rtype: ``bool``


    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.PkiIdentity.organization
        
            The organization as a string (type ``str``).
    
        .. autoattribute:: pyuaf.util.PkiIdentity.organizationUnit
        
            The organization unit as a string (type ``str``).
    
        .. autoattribute:: pyuaf.util.PkiIdentity.locality
        
            The locality as a string (type ``str``).
    
        .. autoattribute:: pyuaf.util.PkiIdentity.state
        
            The state as a string (type ``str``).
    
        .. autoattribute:: pyuaf.util.PkiIdentity.country
        
            The country code as a string (type ``str``).
            
            Note that invalid data (e.g. "Belgium" instead of the correct "BE") will be stored by 
            the PkiIdentity, but this data may get lost when it is converted to a X509 certificate. 
    
        .. autoattribute:: pyuaf.util.PkiIdentity.commonName
        
            The common name as a string (type ``str``).
    
        .. autoattribute:: pyuaf.util.PkiIdentity.domainComponent
        
            The domain component as a string (type ``str``).
    


*class* QualifiedName
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.QualifiedName


    An QualifiedName consists of a name and a corresponding namespace index or namespace URI.


    * Methods:

        .. automethod:: pyuaf.util.QualifiedName.__init__([name[, nameSpaceUri[, nameSpaceIndex]]])
    
            Construct a new QualifiedName in one of the following ways:
            
            .. doctest::
            
                >>> import pyuaf
                >>> from pyuaf.util import QualifiedName
                
                >>> name    = "MyName"
                >>> nsUri   = "myNameSpaceUri"
                >>> nsIndex = 2
                
                >>> # the following ways of constructing a QualifiedName are possible:
                >>> qName0 = QualifiedName()                        # incomplete, no information given!
                >>> qName1 = QualifiedName(name)                    # incomplete, no namespace information! 
                >>> qName2 = QualifiedName(name, nsUri)             # OK, fully qualified 
                >>> qName3 = QualifiedName(name, nsIndex)           # OK, fully qualified 
                >>> qName4 = QualifiedName(name, nsUri, nsIndex)    # OK, fully qualified 
                
                >>> # strings are assumed to be UTF-8 encoded!
                >>> unicodeObject = u"40\u00B0 Celsius ist eine hei\u00DFe Temperatur!"
                >>> qName5 = QualifiedName(unicodeObject.encode("UTF-8"), nsUri)
    
    
        .. automethod:: pyuaf.util.QualifiedName.hasNameSpaceUri
        
            Check if a non-empty namespace URI was given.
            
            :return: True if this QualifiedName contains a non-empty server URI, False if not.
            :rtype:  ``bool``
    
    
        .. automethod:: pyuaf.util.QualifiedName.hasNameSpaceIndex
        
            Check if a namespace index was given.
            
            :return: True if a namespaceIndex has been defined, False if not.
            :rtype:  ``bool``

    
        .. automethod:: pyuaf.util.QualifiedName.name
        
            Get the name part of the QualifiedName.
            
            :return: The name part.
            :rtype:  ``str``

    
        .. automethod:: pyuaf.util.QualifiedName.nameSpaceUri
        
            Get the namespace URI of the QualifiedName.
          
            This function will always return a string, so call hasNameSpaceUri() to see if it's a
            valid one.
            
            :return: The namespace URI.
            :rtype:  ``str``

    
        .. automethod:: pyuaf.util.QualifiedName.nameSpaceIndex
        
            Get the namespace index of the QualifiedName.
          
            This function will always return an ``int``, so call hasNameSpaceIndex() to see if it's
            a valid one.
            
            :return: The namespace index.
            :rtype:  ``int``

    
        .. automethod:: pyuaf.util.QualifiedName.setName(name)
        
            Set the name part of the QualifiedName.
            
            :param name: The name part.
            :type  name: ``str``

    
        .. automethod:: pyuaf.util.QualifiedName.setNameSpaceUri(uri)
        
            Set the namespace URI of the QualifiedName.
            
            :param uri: The namespace URI.
            :type  uri:  ``str``

    
        .. automethod:: pyuaf.util.QualifiedName.setNameSpaceIndex(index)
        
            Set the namespace index of the QualifiedName.
            
            :param uri: The namespace index.
            :type  uri:  ``int``





*class* QualifiedNameVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.QualifiedNameVector

    A QualifiedNameVector is a container that holds elements of type 
    :class:`pyuaf.util.QualifiedName`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`~pyuaf.util.QualifiedName`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import QualifiedNameVector, QualifiedName
        
        >>> # construct a QualifiedNameVector without elements:
        >>> vec = QualifiedNameVector()
        
        >>> noOfElements = len(vec) # will be 0
        
        >>> vec.append(QualifiedName("some name", "/some/URI/"))
        
        >>> noOfElements = len(vec) # will be 1
        
        >>> vec.resize(2)
        
        >>> vec[1].setName("Some other name")
        >>> vec[1].setNameSpaceUri("/some/other/URI/")
        
        >>> noOfElements = len(vec) # will be 2
        
        >>> # you may construct a QualifiedNameVector from a regular Python list:
        >>> someOtherVec = QualifiedNameVector( [ QualifiedName("a", "uriA"), 
        ...                                       QualifiedName("b", "uriB") ] )



*class* ReferenceDescription
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ReferenceDescription

    A ReferenceDescription instance describes a Reference to a specific Node in some address
    space.


    * Methods:


        .. automethod:: pyuaf.util.ReferenceDescription.__init__
    
            Construct a new ReferenceDescription.

    
        .. automethod:: pyuaf.util.ReferenceDescription.__str__
    
            Get a string representation of the ReferenceDescription.


    * Attributes:

      
        .. autoattribute:: pyuaf.util.ReferenceDescription.referenceTypeId
        
            The NodeId (as a :class:`pyuaf.util.NodeId`) of the type of the reference.
  
        .. autoattribute:: pyuaf.util.ReferenceDescription.isForward
        
            A ``bool``: ``True`` if the server followed a forward reference, ``False`` if not.
  
        .. autoattribute:: pyuaf.util.ReferenceDescription.nodeId
        
            The ExpandedNodeId (as a :class:`pyuaf.util.ExpandedNodeId`) of the node to which 
            the reference is pointing.
        
        .. autoattribute:: pyuaf.util.ReferenceDescription.browseName
        
            The browse name (as a :class:`pyuaf.util.QualifiedName`) of the node to which the
            reference is pointing.
        
        .. autoattribute:: pyuaf.util.ReferenceDescription.displayName
        
            The display name (as a :class:`pyuaf.util.LocalizedText`) of the node to which 
            the reference is pointing.
        
        .. autoattribute:: pyuaf.util.ReferenceDescription.nodeClass
        
            The node class of the node to which the reference is pointing,
            as an ``int`` as defined in the :mod:`pyuaf.util.nodeclasses` module.
  
        .. autoattribute:: pyuaf.util.ReferenceDescription.typeDefinition
        
            The ExpandedNodeId (as a :class:`pyuaf.util.ExpandedNodeId`) of the type of the node 
            to which the reference is pointing to (only in case the node class of this node is 
            an Object or a Variable).
    



*class* ReferenceDescriptionVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ReferenceDescriptionVector

    A ReferenceDescriptionVector is a container that holds elements of type 
    :class:`pyuaf.util.ReferenceDescription`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`~pyuaf.util.ReferenceDescription`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import ReferenceDescriptionVector, ReferenceDescription, ExpandedNodeId
        
        >>> # construct a ReferenceDescriptionVector without elements:
        >>> vec = ReferenceDescriptionVector()
        
        >>> noOfElements = len(vec) # will be 0
        
        >>> vec.append(ReferenceDescription())
        
        >>> noOfElements = len(vec) # will be 1
        
        >>> vec.resize(2)
        
        >>> vec[1].nodeId    = ExpandedNodeId("SomeId", "SomeNameSpace", "SomeServerUri")
        >>> vec[1].nodeClass = pyuaf.util.nodeclasses.Variable
        
        >>> noOfElements = len(vec) # will be 2
        
        >>> # you may construct a ReferenceDescriptionVector from a regular Python list:
        >>> someOtherVec = ReferenceDescriptionVector( [ ReferenceDescription(), 
        ...                                              ReferenceDescription() ] )



*class* RelativePath
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.RelativePath

    A RelativePath is a container that holds elements of type 
    :class:`pyuaf.util.RelativePathElement`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`pyuaf.util.RelativePathElement`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import RelativePath, RelativePathElement
        
        >>> # construct a RelativePath without elements:
        >>> path = RelativePath()
        
        >>> noOfElements = len(path) # will be 0
        
        >>> path.append(RelativePathElement())
        
        >>> noOfElements = len(path) # will be 1
        
        >>> path.resize(4)
        
        >>> noOfElements = len(path) # will be 4
        
        >>> element0 = path[0]
        
        >>> # you may construct a RelativePath from a regular Python list:
        >>> someOtherPath = RelativePath( [RelativePathElement(), RelativePathElement()] )



*class* RelativePathElement
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.RelativePathElement


    A RelativePathElement describes one element of a RelativePath and basically contains a target 
    (a qualified name), the type of the reference to this target, and some other properties.
    

    * Methods:

        .. automethod:: pyuaf.util.RelativePathElement.__init__
    
            Construct a NodeIdIdentifier by providing a string or a numeric value (or nothing at 
            all).
        
            You can construct a RelativePathElement like this:
            
            .. doctest::
            
                >>> import pyuaf
                >>> from pyuaf.util                  import RelativePathElement, QualifiedName, NodeId
                >>> from pyuaf.util.opcuaidentifiers import OpcUaId_HierarchicalReferences
            
                >>> elem0 = RelativePathElement()
                >>> elem1 = RelativePathElement(QualifiedName("SomeBrowseName", 42))
                >>> elem2 = RelativePathElement(QualifiedName("SomeBrowseName", 42), NodeId(OpcUaId_HierarchicalReferences, 0), True, False)
        

    * Attributes:
    
        .. autoattribute:: pyuaf.util.RelativePathElement.targetName
        
            The target name (a :class:`~pyuaf.util.QualifiedName`).
    
        .. autoattribute:: pyuaf.util.RelativePathElement.referenceType
        
            The reference type as a :class:`~pyuaf.util.NodeId` (which may, or may not be resolved).
    
        .. autoattribute:: pyuaf.util.RelativePathElement.isInverse
        
            Flag specifying whether or not the inverse reference is meant (as a ``bool``).
    
        .. autoattribute:: pyuaf.util.RelativePathElement.includeSubtypes
        
            Flag specifying if subtypes of the reference type should be included (as a ``bool``).


*class* SdkStatus
----------------------------------------------------------------------------------------------------

.. autoclass:: pyuaf.util.SdkStatus

    An SdkStatus stores the information (i.e. status code and message) of a Status object
    by the SDK.


    * Methods:
    
        .. automethod:: pyuaf.util.SdkStatus.__init__
    
            Construct a new SdkStatus.


        .. automethod:: pyuaf.util.SdkStatus.isGood
        
            Check if the status has a "good" status code.
            
            :return: True if the status is Good.
            :rtype:  bool
    
    
        .. automethod:: pyuaf.util.SdkStatus.isNotGood
        
            Check if the status does not have a "good" status code.
            
            :return: True if the status is not Good.
            :rtype:  bool
    
    
        .. automethod:: pyuaf.util.SdkStatus.isUncertain
        
            Check if the status has an "uncertain" status code.
            
            :return: True if the status is Uncertain.
            :rtype:  bool
    
    
        .. automethod:: pyuaf.util.SdkStatus.isNotUncertain
        
            Check if the status does not have a "uncertain" status code.
            
            :return: True if the status is not Uncertain.
            :rtype:  bool
    
    
        .. automethod:: pyuaf.util.SdkStatus.isBad
        
            Check if the status has a "bad" status code.
            
            :return: True if the status is Bad.
            :rtype:  bool
    
    
        .. automethod:: pyuaf.util.SdkStatus.isNotBad
        
            Check if the status does not have a "bad" status code.
            
            :return: True if the status is not Bad.
            :rtype:  bool


    * Attributes:
    
        .. autoattribute:: pyuaf.util.SdkStatus.statusCode
        
            Get the OPC UA status code.
            
            *OPC UA status codes* are not the same as *UAF status codes*! 
            See the :mod:`pyuaf.util.opcuastatuscodes` and :mod:`pyuaf.util.statuscodes` modules
            for more info!
            
            :return: The OPC UA status code, as defined by :mod:`pyuaf.util.opcuastatuscodes`.
            :rtype:  ``int``

        .. autoattribute:: pyuaf.util.SdkStatus.message
        
            Get the message of the SDK status.
            
            :return: The message of the status.
            :rtype:  ``str``



*class* SimpleAttributeOperand
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.SimpleAttributeOperand

    A SimpleAttributeOperand is part of a filter and can describe select clauses, where clauses, etc.


    * Methods:

        .. automethod:: pyuaf.util.SimpleAttributeOperand.__init__
    
            Construct a new SimpleAttributeOperand.
    
        .. automethod:: pyuaf.util.SimpleAttributeOperand.__str__
    
            Get a string representation of the SimpleAttributeOperand.


    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.SimpleAttributeOperand.attributeId
        
            The attribute id (e.g. :attr:`pyuaf.util.attributeids.Value`) as an ``int`` as 
            defined in :mod:`pyuaf.util.attributeids`.
  
  
        .. autoattribute:: pyuaf.util.SimpleAttributeOperand.browsePath
        
            A "simple" browse path (actually just a list of qualified names),
            as a :class:`~pyuaf.util.QualifiedNameVector`.
  
  
        .. autoattribute:: pyuaf.util.SimpleAttributeOperand.typeId
        
            The type id, as a :class:`~pyuaf.util.NodeId`.
  
  
        .. autoattribute:: pyuaf.util.SimpleAttributeOperand.indexRange
        
            The index range of an array, as a ``str``.
            
    


*class* SimpleAttributeOperandVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.SimpleAttributeOperandVector

    A SimpleAttributeOperandVector is a container that holds elements of type 
    :class:`pyuaf.util.SimpleAttributeOperand`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`pyuaf.util.SimpleAttributeOperand`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import SimpleAttributeOperandVector, \
        ...                        SimpleAttributeOperand, \
        ...                        QualifiedName, \
        ...                        NodeId
        
        >>> # construct a SimpleAttributeOperandVector without elements:
        >>> vec = SimpleAttributeOperandVector()
        
        >>> noOfElements = len(vec) # will be 0
        
        >>> vec.append(SimpleAttributeOperand())
        
        >>> vec[0].attributeId = pyuaf.util.attributeids.Value
        >>> vec[0].browsePath.append(QualifiedName("some name", 3))
        >>> vec[0].typeId = NodeId(pyuaf.util.opcuaidentifiers.OpcUaId_BaseEventType, 0)
        
        >>> noOfElements = len(vec) # will be 1
        
        >>> vec.resize(4)
        
        >>> noOfElements = len(vec) # will be 4
        
        >>> # you may construct a SimpleAttributeOperandVector from a regular Python list:
        >>> someOtherVec = SimpleAttributeOperandVector( [ SimpleAttributeOperand(), 
        ...                                                SimpleAttributeOperand() ] )





*class* Status
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.Status


     A Status object holds a UAF status code, a message and an OPC UA status code.

    * Attributes:
    
        .. autoattribute:: pyuaf.util.Status.statusCode
        
            The UAF status code (an ``int`` as defined by :mod:`pyuaf.util.statuscodes`).
            
            UAF status codes are not the same as OPC UA status codes! 
            For each UAF status code (except for Good and Uncertain), there is a corresponding 
            error.
            See the :mod:`pyuaf.util.statuscodes` and :mod:`pyuaf.util.opcuastatuscodes` modules
            for more info!    

    * Methods:

        .. automethod:: pyuaf.util.Status.__init__
        
            Construct a Status by providing a UAF status code (as defined by 
            :mod:`pyuaf.util.statuscodes`).
            
            UAF Status objects are Uncertain by default.
        
            You can construct a Status like this:
            
            .. doctest::
            
                >>> import pyuaf
                >>> from pyuaf.util import Status, statuscodes
                >>> from pyuaf.util.errors import InvalidServerUriError
            
                >>> status0 = Status() # Uncertain by default!
                >>> status1 = Status(statuscodes.Good)
                >>> status2 = Status(InvalidServerUriError("http://invalid/server/uri"))
                
        
        .. automethod:: pyuaf.util.Status.test()
        
            Important method: raise an error if the status is Bad! E.g. 
            if `~pyuaf.util.Status.statusCode` equals `pyuaf.util.statuscodes.InvalidServerUriError`,
            then a `pyuaf.util.errors.InvalidServerUriError` will be raised!
            If the `~pyuaf.util.Status.statusCode` attribute is `~pyuaf.util.statuscodes.Good` 
            or `~pyuaf.util.statuscodes.Uncertain`, nothing will happen when you call this method.
            
            
        .. automethod:: pyuaf.util.Status.setGood()
        
            Set the status to Good.
            
        
        .. automethod:: pyuaf.util.Status.setUncertain()
        
            Set the status to Uncertain.
        
    
        .. automethod:: pyuaf.util.Status.isGood
        
            Check if the status has a "good" status code.
            
            :return: True if the status is Good.
            :rtype:  bool
    
    
        .. automethod:: pyuaf.util.Status.isNotGood
        
            Check if the status does not have a "good" status code.
            
            :return: True if the status is not Good.
            :rtype:  bool
    
    
        .. automethod:: pyuaf.util.Status.isUncertain
        
            Check if the status has an "uncertain" status code.
            
            :return: True if the status is Uncertain.
            :rtype:  bool
    
    
        .. automethod:: pyuaf.util.Status.isNotUncertain
        
            Check if the status does not have a "uncertain" status code.
            
            :return: True if the status is not Uncertain.
            :rtype:  bool
    
    
        .. automethod:: pyuaf.util.Status.isBad
        
            Check if the status has a "bad" status code.
            
            :return: True if the status is Bad.
            :rtype:  bool
    
    
        .. automethod:: pyuaf.util.Status.isNotBad
        
            Check if the status does not have a "bad" status code.
            
            :return: True if the status is not Bad.
            :rtype:  bool
    
        
        .. automethod:: pyuaf.util.Status.statusCodeName
        
            Get the name of the UAF status code.
            
            UAF status codes are not the same as OPC UA status codes! 
            See the :mod:`pyuaf.util.statuscodes` and :mod:`pyuaf.util.opcuastatuscodes` modules
            for more info!
            
            :return: The name of the UAF status code.
            :rtype:  ``str``
    
        .. automethod:: pyuaf.util.Status.summarize(statuses)
        
            Make a "summary" of other statuses.
            
            Bad is dominant to Uncertain, which is dominant to Good. 
            So if any Bad status is present, the summary will be "Bad".
            If no Bad statuses are present but any Uncertain status is present, the summary will be
            Uncertain. And if only Good statuses are present, the summary will be Good.
            
            :param statuses: The statuses to summarize.
            :type  statuses: :class:`pyuaf.util.StatusVector`.

    
        .. automethod:: pyuaf.util.Status.isRaisedBy
        
            Check if this (bad!) status is raised by some other status.
            
            :return: True if this status was raised by some other status.
            :rtype:  bool

    
        .. automethod:: pyuaf.util.Status.raisedBy
        
            If this status is raised by some other status, get a copy of this other status.
            It only make sense to call this method when `~pyuaf.util.Status.isRaisedBy` returns True.
            
            :return: Get a copy of the status that raised this status.
            :rtype:  `~pyuaf.util.Status`


        .. automethod:: pyuaf.util.Status.setRaisedBy
        
            Store the status that caused this status (i.e. the current instance).
            
            :param status: The status that caused this (bad!) status.
            :type status: `~pyuaf.util.Status`
            
            

*class* StatusVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.StatusVector

    A StatusVector is a container that holds elements of type  :class:`pyuaf.util.Status`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`pyuaf.util.Status`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import Status, StatusVector
        
        >>> # construct a StatusVector without elements:
        >>> statuses = StatusVector()
        
        >>> noOfElements = len(statuses) # will be 0
        
        >>> statuses.append(Status())
        
        >>> noOfElements = len(statuses) # will be 1
        
        >>> statuses.resize(4)
        
        >>> noOfElements = len(statuses) # will be 4
        
        >>> element0 = statuses[0]
        
        >>> # you may construct a StatusVector from a regular Python list:
        >>> otherStatuses = StatusVector( [Status(), Status()] )




*class* StringVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.StringVector

    A StringVector is a container that holds string elements. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of string.

    Usage example:
    
    .. literalinclude:: /../../../examples/pyuaf/util/how_to_use_a_stringvector.py




*class* UInt32Vector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.UInt32Vector

    An UInt32Vector is a container that holds unsigned 32-bit ``int`` elements 
    (not :class:`~pyuaf.util.primitives.UInt32` objects!)
    
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of ``int``.
    
    An :class:`~pyuaf.util.UInt32Vector` is exactly the same as a :class:`~pyuaf.util.StringVector`, 
    except of course that it holds ``int`` elements instead of ``str`` elements. 
    For an elaborate example, check out the example at the :class:`~pyuaf.util.StringVector` 
    documentation.



*class* UserTokenPolicy
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.UserTokenPolicy

    A UserTokenPolicy instance describes the type, ID, issued token type, etc. of a user token.


    * Methods:

        .. automethod:: pyuaf.util.UserTokenPolicy.__init__
    
            Construct a new UserTokenPolicy.
    
    
    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.UserTokenPolicy.policyId
        
            The policy ID as a ``str``, as defined in :mod:`pyuaf.util.securitypolicies`.
            
            By default, the policyId is :attr:`~pyuaf.util.securitypolicies.UA_None`.
    
        .. autoattribute:: pyuaf.util.UserTokenPolicy.tokenType
        
            The token type (e.g. :attr:`~pyuaf.util.usertokentypes.UserName`)
            (an ``int``, as defined in :mod:`pyuaf.util.usertokentypes`).
            
            By default, the tokenType is :attr:`~pyuaf.util.usertokentypes.Anonymous`.
    
        .. autoattribute:: pyuaf.util.UserTokenPolicy.issuedTokenType
        
            The issued token type as a ``str``.
    
        .. autoattribute:: pyuaf.util.UserTokenPolicy.issuerEndpointUrl
        
            The issuer endpoint URL as a ``str``.
    
        .. autoattribute:: pyuaf.util.UserTokenPolicy.securityPolicyUri
        
            The security policy URI as a ``str``.



*class* UserTokenPolicyVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.UserTokenPolicyVector

    A QualifiedNameVector is a container that holds elements of type 
    :class:`~pyuaf.util.UserTokenPolicy`. 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of :class:`~pyuaf.util.UserTokenPolicy`.

    Usage example:
    
    .. doctest::
    
        >>> import pyuaf
        >>> from pyuaf.util import UserTokenPolicyVector, UserTokenPolicy
        
        >>> # construct a UserTokenPolicyVector without elements:
        >>> vec = UserTokenPolicyVector()
        
        >>> noOfElements = len(vec) # will be 0
        
        >>> vec.append(UserTokenPolicy())
        >>> vec[0].policyId = pyuaf.util.securitypolicies.UA_Basic128Rsa15
        
        >>> noOfElements = len(vec) # will be 1
        
        >>> vec.resize(2)
        >>> vec[1].policyId = pyuaf.util.securitypolicies.UA_Basic256Rsa15
        
        >>> noOfElements = len(vec) # will be 2
        
        >>> # you may construct a UserTokenPolicyVector from a regular Python list:
        >>> someOtherVec = UserTokenPolicyVector( [ UserTokenPolicy(), UserTokenPolicy() ] )





     
*class* VariantVector
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.VariantVector

    A VariantVector is a container that holds variants (so variables with a dynamic data type). 
    It is an artifact automatically generated from the C++ UAF code, and has the same functionality
    as a ``list`` of values, where the values can be instances of:
    
      - ``None`` (for a NULL value)
      - a primitive:
         - :class:`pyuaf.util.primitives.Boolean`
         - :class:`pyuaf.util.primitives.SByte`
         - :class:`pyuaf.util.primitives.Byte`
         - :class:`pyuaf.util.primitives.UInt16`
         - :class:`pyuaf.util.primitives.Int16`
         - :class:`pyuaf.util.primitives.UInt32`
         - :class:`pyuaf.util.primitives.Int32`
         - :class:`pyuaf.util.primitives.UInt64`
         - :class:`pyuaf.util.primitives.Int64`
         - :class:`pyuaf.util.primitives.Float`
         - :class:`pyuaf.util.primitives.Double`
         - :class:`pyuaf.util.primitives.String`
         - :class:`pyuaf.util.primitives.ByteString`
      - a non-primitive such as:
         - :class:`pyuaf.util.NodeId`
         - :class:`pyuaf.util.ExpandedNodeId`
         - :class:`pyuaf.util.QualifiedName`
         - :class:`pyuaf.util.LocalizedText`
         - :class:`pyuaf.util.DateTime`
      - a ``list`` of any of the above types. This list represents an OPC UA array, so all items
        of this list should have the same type!!!
    
    .. seealso:: :ref:`note-variants`.

    Usage example:
    
    .. literalinclude:: /../../../examples/pyuaf/util/how_to_use_a_variantvector.py








*class* ViewDescription
----------------------------------------------------------------------------------------------------


.. autoclass:: pyuaf.util.ViewDescription


    A ViewDescription instance specifies an OPC UA View.


    * Methods:

        .. automethod:: pyuaf.util.ViewDescription.__init__
    
            Construct a new ViewDescription.
            
    
    * Attributes:
  
  
        .. autoattribute:: pyuaf.util.ViewDescription.viewId

            The node id of the View to query.
            
            Leave the NodeId to its default value (= without any contents, so NodeId.isNull()
            will return True) if you want to specify the whole address space.


