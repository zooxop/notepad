> 출처 : https://gist.github.com/stropdale/7de7a1c25dcf9316b223d3364c60b168#file-dnslookup-swift-L4

```swift
import UIKit

// You many want to run this in the background
func getIPs(dnsName: String) -> String? {
    let host = CFHostCreateWithName(nil, dnsName as CFString).takeRetainedValue()
    CFHostStartInfoResolution(host, .addresses, nil)
    var success: DarwinBoolean = false
    if let addresses = CFHostGetAddressing(host, &success)?.takeUnretainedValue() as NSArray? {
        for case let theAddress as NSData in addresses {
            var hostname = [CChar](repeating: 0, count: Int(NI_MAXHOST))
            if getnameinfo(theAddress.bytes.assumingMemoryBound(to: sockaddr.self), socklen_t(theAddress.length),
                           &hostname, socklen_t(hostname.count), nil, 0, NI_NUMERICHOST) == 0 {
                let numAddress = String(cString: hostname)
                return numAddress
            }
        }
    }
    
    return nil
}

print(getIPs(dnsName: "HostName"))
```