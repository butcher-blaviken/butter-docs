

## Initialize

When initiating omni-chain on the source chain contract, it is necessary to introduce the IMOSV3 interface. 
You can directly import the protocol using the following code, but make sure to install the protocol with 
```'npm install @butternetwork/omniservice``` 
before usage.

```
@butternetwork/omniservice/contracts/interface/IMOSV3.sol;
```

### Estimate omni-chain message fee

Before initiating a cross-chain transaction, it is necessary to estimate the cross-chain messaging fee.
The fee structure for omni-chain message can be referenced here [Omnichain Fee](Omnichain-Fee.md).

The interface for estimating the fee is
```solidity
function getMessageFee(uint256 toChain, address feeToken, uint256 gasLimit) external view returns (uint256 fee, address receiver);
```

- `toChain` - Target chain id, get [target chain id](deployed-omnichain-contracts.md) here.
- `feeToken` - Token address that supports payment fee, native token is `address(0)`.

    Currently, the only supported is native token, and more tokens will be supported in the future.
- `gasLimit` - The gasLimit allowed to be consumed by an operation performed on the target chain.


## Initiate omni-chain message

When initiating a omni-chain request, in addition to the data you wish to execute across chains, we also provide various applicable scenarios for users to flexibly choose the most suitable method according to their own needs.

```
    enum MessageType {
        CALLDATA,
        MESSAGE
    }

    struct MessageData {
        bool relay;
        MessageType msgType;
        bytes target;
        bytes payload;
        uint256 gasLimit;
        uint256 value;
    }
```

Check the structure of MessageData.

- `relay` indicates whether message processing is required on MAP Relay Chain. 
  - If `relay` is false, during the message passing through the relay chain (Mapo), no additional processing is done, and the message is directly sent to the target chain.
  - If `relay` is true, the message will be processed on relay chain before forwarding to the target chain. Check [Message And Relay](./message-relay.md) for more details.
- `msgType` indicates different message
  - `MESSAGE` allows for freely defined omni-chain messages. When they reach the target chain, the cross-chain messages are passed to the `mapoExecute` method. You can define your own preferred checking methods and data processing in this method to complete the cross-chain message execution.
  - `CALLDATA` requires the complete `calldata` to be prepared on the source chain for execution on the target. OmniService contract should be granted permissions to execute the call.
- `target` is the contract address where the message will be executed upon reaching the target chain
- `payload` is the data intended for cross-chain transmission.
- `gasLimit` is the maximum gas limit allowed for execution on the target chain.
- `value` should currently be defaulted to 0; further details will be forthcoming.

After understanding the various options available for `MessageData`, we can directly encode the assembled `MessageData` and then call `transferOut` to send the cross-chain request.

```
    bytes memory messageData = abi.encode(MessageData({}));
    
    function transferOut(
        uint256 toChain,
        bytes memory messageData,
        address feeToken
    ) external payable returns (bytes32);
```

## Execute the omni-chain message

When message reaches the target chain, it will be executed according to the choice made during cross-chain initiation, and emit an event upon completion of execution. 
Depending on the `msgType` chosen freely during cross-chain initiation, we will employ different handling methods on the target chain.

### Message execute
- The `MESSAGE` mode offers greater freedom and broader adaptability, but requires users to implement the following interfaces on the target chain.

  ```
      function mapoExecute(
          uint256 _fromChain,
          uint256 _toChain,
          bytes calldata _fromAddress,
          bytes32 _orderId,
          bytes calldata _message
      ) external returns (bytes memory newMessage)
  ```

  If you want to facilitate the implementation of the `mapoExecute` method, you can directly install the `@butternetwork/omniservice` protocol and import the `IMapoExecutor` interface using the following code:

  ```
  import "@butternetwork/omniservice/contracts/interface/IMapoExecutor.sol";
  ```

  The `mapoExecute` method is flexible. We will pass the information from the source chain along with your custom message. You can freely define the validation rules, including decoding the message, among other things. This method is suitable for a wide range of scenarios.

### Contract call

- The `CALLDATA` mode involves preparing the calldata for execution on the target chain at the time of initiating the cross-chain request. The `OmniService` will directly execute the call and then emit an event upon completion.

Of course, we also consider that there are many different chains currently. Because data cannot perfectly intercommunicate between chains, we have created the Butter Omnichain Service to accomplish this great feat. Each chain has its own characteristics, and occasional execution failures are inevitable. But don't worry, even if cross-chain execution fails, we will save the hash of the failed execution. You can retrieve the information for re-execution through the transaction logs, allowing you to correct the execution logic and attempt the cross-chain message execution again. For more details, please see below:


### Message execution retry


```
    function retryMessageIn(
        uint256 _fromChain,
        bytes32 _orderId,
        bytes calldata _fromAddress,
        bytes calldata _messageData
    ) external 
```

`retryMessageIn` is flexible and can be called with any correct data for execution. 
It does not alter the cross-chain message, as we save the hash at the time of failure and will perform hash validation. 
It simply provides more opportunities for attempts, making cross-chain communication more free and seamless.

### How to choose MessageType
[MESSAGE](Omnichain-Type/Message-Type.md) : MESSAGE type is flexible and highly extensible, making it suitable for handling various types of cross-chain messages. We highly recommend using this type.

[Message And Relay](Omnichain-Type/MessageAndRelayTrue-Type.md) : Of course, if the source chain cannot perfectly handle cross-chain information, you can set the relay attribute of MessageData to true. This way, during the cross-chain process, the relay chain can perform data processing or enhance the cross-chain data before continuing.

[CALLDATA](Omnichain-Type/Calldata-Type.md) : When you can clearly and effectively determine the execution method on the target chain from the source chain, you can choose the CALLDATA type. This way, the target chain only needs to grant permission, making cross-chain execution straightforward.



