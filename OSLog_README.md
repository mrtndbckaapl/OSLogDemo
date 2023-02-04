#   OSLog usage

###  creation of logger and simple log:

```
    import os
    let logger = Logger(subsystem: "com.example.YourBundeId", category: "networking")
    logger.log("Database launched successfully")
```

 - subsystem: Usually bundleId of application
 - category: any category tag needed, e.g. networking, bluetooth, CoredataManager etc.
 
 
 ###  logging any CustomStringConvertible, where non-numeric types are redacted by default not showing sensitive data:
 `
    logger.log( "Paid with bank account \ (accountNumber)")
 `
 
output in console: 
09:41:00-868918-0700  YourBundeId  Paid with bank account <private>

-hence `accountNumber` is of `String` Type (nonnumeric), the value is private when used on realease builds. 
-if `accountNumber` was  of numeric Type such as `Int` or `Double`  the value would not be private when used on realease builds. 


 ###  logging any CustomStringConvertible, where non-numeric types are publicly seen:
 `
    logger.log("Ordered smoothie \ (smoothieName, privacy: .public)")
 `
 
output in console:
09:41:00-868918-0700  YourBundeId  Ordered smoothie blueberry

 ###  syntax for logging error message:
 `
    logger.error( "invalid token with id: \(tokenIdString)")
 `
 
  ###  syntax for logging fault message:
 `
    logger.fault( "invalid token with id: \(tokenIdString)")
 `


 ##  Retrieving logs from archive example:

 `
 sudo log collect --device-name="Martin’s iPhone" --start '2023-01-05 07:28:00' --output OSLogDemo.logarchive
 `


 source: WWDC 2020: https://developer.apple.com/videos/play/wwdc2020/10168/
 
 
 
