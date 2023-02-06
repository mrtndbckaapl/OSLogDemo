# OSLog usage:

### Minimum deployment: of `Logger` in `os` library available iOS 14+ , `OSLog` library available iOS 15+
### 1) creation of logger and simple log:

```
    import os
    let logger = Logger(subsystem: "com.example.YourBundeId", category: "networking")
    logger.log("Database launched successfully")
```

 - subsystem: Usually bundleId of application
 - category: any category tag needed, e.g. networking, bluetooth, CoredataManager etc.
 
 
 ###  2) logging any CustomStringConvertible, where non-numeric types are redacted by default not showing sensitive data:
 `
    logger.log( "Paid with bank account \ (accountNumber)")
 `
 
***output in console:*** 
> 09:41:00-868918-0700  YourBundeId  Paid with bank account \<private\>

- hence `accountNumber` is of `String` Type (nonnumeric), the value is private when used on realease builds. 
- if `accountNumber` was  of numeric Type such as `Int` or `Double`  the value would not be private when used on realease builds. 


 ###  3) logging any CustomStringConvertible, where non-numeric types are publicly seen:
 `logger.log("Ordered smoothie \ (smoothieName, privacy: .public)")`
 
***output in console***
> 09:41:00-868918-0700  YourBundeId  Ordered smoothie blueberry

 ###  4) syntax for logging error message:
 `logger.error( "invalid token with id: \(tokenIdString)")`
 
  ### 5) syntax for logging fault message:
 `logger.fault( "invalid token with id: \(tokenIdString)")`

 ##  Retrieving logs :
 
 ###  a) Retrieving logs from archive example:

  `sudo log collect --device-name="Martin’s iPhone" --start '2023-01-05 07:28:00' --output OSLogDemo.logarchive`
  
  ###  b) Retrieving logs withing app:
```
import OSLog

class LogStore {
    func retrieveLogs() -> [String] {
        let logStore = try! OSLogStore(scope: .currentProcessIdentifier)
        let yesterday = Date().addingTimeInterval(-3600 * 24)
        let timeFrame = logStore.position(date: yesterday)
        let allEntries = try! logStore.getEntries(at: timeFrame)
        let logMessages: [String] = allEntries.compactMap { entry in
            return [entry.date.formatted(),
                    entry.storeCategory.rawValue.formatted(),
                    entry.composedMessage]
                .joined(separator: " ")
        }
        return logMessages
    }
}
```


 
## Source1: [WWDC 2020 Video](https://developer.apple.com/videos/play/wwdc2020/10168/)
## Source2: [CocoaHeads Berlin Video](https://www.youtube.com/watch?v=oHxxkWhOSK0)
## Source3: [Logging with os Documentation](https://developer.apple.com/documentation/os/logging/generating_log_messages_from_your_code)
## Source4: [OSLog Documentation](https://developer.apple.com/documentation/oslog)
 
 
 
