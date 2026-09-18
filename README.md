<img src="https://www.activeledger.io/wp-content/uploads/2018/09/Asset-23.png" alt="Activeledger" width="500"/>


# Activeledger SDK-IOS

> ## ⚠️ Unmaintained and archived
>
> **This SDK is not maintained and should not be used for new work.** It was
> last updated in 2020 and supports neither post-quantum identities
> (`ml-dsa-65`, `falcon-512`) nor `secp256k1` as the ledger encodes it today —
> so it cannot create an identity that a current Activeledger network will
> accept as post-quantum, and cannot sign for one.
>
> It is archived rather than rewritten because nothing currently depends on
> it. If you need a maintained iOS SDK, raise an issue on
> [activeledger](https://github.com/activeledger/activeledger) — it is worth
> building, just not on spec.
>
> ### Maintained SDKs
>
> | Language | Repository | Post-quantum | secp256k1 |
> |---|---|:---:|:---:|
> | JavaScript / TypeScript | [SDK-JS](https://github.com/activeledger/SDK-JS) | ML-DSA-65, Falcon-512 | ✅ |
> | Kotlin / Java / Android | [SDK-JVM](https://github.com/activeledger/SDK-JVM) | ML-DSA-65, Falcon-512 | ✅ |
> | C# / .NET | [SDK-CSharp](https://github.com/activeledger/SDK-CSharp) | ML-DSA-65, Falcon-512 | ✅ |
> | Python | [SDK-Python](https://github.com/activeledger/SDK-Python) | ML-DSA-65, Falcon-512 | ✅ |
> | Go | [SDK-Golang](https://github.com/activeledger/SDK-Golang) | ML-DSA-65 | ✅ |
> | Rust | [SDK-Rust](https://github.com/activeledger/SDK-Rust) | ML-DSA-65 | ✅ |
> | PHP | [SDK-PHP](https://github.com/activeledger/SDK-PHP) | ML-DSA-65 | ✅ |
>
> A Swift client can also drive a node over HTTP directly: a transaction is
> JSON, and the signature covers the exact bytes of `JSON.stringify($tx)`.
>
> The companion demo app is
> [SDK-IOS-Example](https://github.com/activeledger/SDK-IOS-Example).


![](https://github.com/activeledger/SDK-IOS/blob/master/assets/appVideo.gif)


## Adding SDK using Cocoa Pods.

Add     pod 'Activeledger-SDK-IOS', '~> 0.1.3'  to projects Pod file.


## SDK Dev Instruction

Use the ActiveLedger SDK interface to Use the SDK.

## Initialise the SDK

```Swift
let activeledgerSDK = ActiveledgerSDK(http: "http", baseURL: "testnet-uk.activeledger.io", port: "5260")
```

## Generate KeyPair

```Swift
activeledgerSDK?.generateKeys(type: encryptionSelected, name: "ASL")
```

## Fetching Public Key in PEM

```Swift
let publicKey = activeledgerSDK?.getPublicKeyPEM()
```

## Fetching Private Key in PEM

```Swift
let privateKey = activeledgerSDK?.getPrivateKeyPEM()
```

## Oboard KeyPair

Onboarding a KeyPair will give an Single Observable in return. Use RxSwift to subscribe to it.

```Swift
let response: Single<JSON> = activeledgerSDK?.onBoardKeys()
```

## Server Sent Event

Server Sent Events can be subscribed by giving the URL. Function return RxSwift Observable that can be subscribed (Example given below). Events are connected automatically and is disconnected when disposing Observable. Dont forget to dispose Observable to avoid memory leaks. 
There are Three events :
1- Open
2- Message
3- Complete

Properties of event object might change with respect to differnt types which can be accessed by "type" property of the event.
In case in which event is disconnect unexpecteldly, User can reconnect using 

```Swift
activeledgerSDK?.reConnectEvent()
```
Example: 

```Swift
activeledgerSDK?.subscribeToEvent(url: URL(string: "http://testnet-uk.activeledger.io:5261/api/activity/subscribe")!)
.subscribe(
                         onNext: { data in
                           print("---event---")
                           print(data.type)

                         },
                         onError: { error in
                           print(error)
                         },
                         onCompleted: {
                           print("Completed")
                         },
                         onDisposed: {
                           print("Disposed")
                           self.activeledgerSDK?.disconnectEvent()
                         }
                     ).disposed(by: bag)
```

## Executing a Transaction

Execute method takes a transaction and will give an Single Observable in return with response in JSON format. Use RxSwiftto subscribe to it.

```Swift
let request: Single<JSON> = executeTransaction(transaction: transaction)

request
.subscribe(onSuccess: { response in

    print("---success response---")
    print(response)
    
}, onError: { error in
    print(error)
}).disposed(by: bag)

```


## License

---

This project is licensed under the [MIT](https://github.com/activeledger/SDK-IOS/blob/master/LICENSE) License

