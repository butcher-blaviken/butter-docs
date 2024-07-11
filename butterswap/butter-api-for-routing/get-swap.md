# GET /swap

GET generated swap transaction calldata to swap in Butter router

#### Params

| Name     | Location | Type   | Required | Description                                                       |
| -------- | -------- | ------ | -------- | ----------------------------------------------------------------- |
| hash     | query    | string | yes      | the route hash returned by /route                                 |
| slippage | query    | string | yes      | slippage of swap, a integer in rang \[0, 5000], e.g, 100 means 1% |
| from     | query    | string | yes      | sender address on source chain                                    |
| receiver | query    | string | yes      | receiver address on destination chain                             |
| callData | query    | string | no       | encoded call data if receiver is a contract                       |

### Request Example

```bash
GET /swap?hash=0x286081342b93d276381a8cf4b43990e3a522fb25d0c50c3467dce7ff4543c92c&slippage=100&from=0x2D4C407BBe49438ED859fe965b140dcF1aaB71a9&receiver=0x2D4C407BBe49438ED859fe965b140dcF1aaB71a9
```

### **Responses**

| HTTP Status Code | Meaning                                                 | Description | Data schema |
| ---------------- | ------------------------------------------------------- | ----------- | ----------- |
| 200              | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | Success     | Inline      |

#### Response Examples

1. get swap transaction calldata successfully

> 200 Response

```json
{
  "errno": 0,
  "message": "string",
  "data": [
    {
      "to": "0x63212C5F70D1b374A023950a96bE3506779cAe24",
      "data": "0x480a341100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000de0b6b3a764000000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000e80000000000000000000000000000000000000000000000000000000000000014000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc2000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc20000000000000000000000002d4c407bbe49438ed859fe965b140dcf1aab71a9000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc20000000000000000000000000000000000000000000000000de0b6b3a764000000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000004d0e30db0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c6000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000038000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000142d4c407bbe49438ed859fe965b140dcf1aab71a90000000000000000000000000000000000000000000000000000000000000000000000000000000000000b8000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000b600000000000000000000000000000000000000000000000000000000000000b00000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004aab00833fd5f13d03115559277b0b8cc0a5ff360000000000000000000000004aab00833fd5f13d03115559277b0b8cc0a5ff360000000000000000000000002d4c407bbe49438ed859fe965b140dcf1aab71a90000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000660b444480037c7000000000000000000000000000000000000000000000000000000000000000e000000000000000000000000000000000000000000000000000000000000009c4efa0646500000000000000000000000000000000000000000000000000000000000000200000000000000000000000002170ed0880ac9a755fd29b2688956bd959f933f8000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002d4c407bbe49438ed859fe965b140dcf1aab71a9000000000000000000000000000000000000000000000000660b444480037c7000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000480000000000000000000000000000000000000000000000000000000000000000500000000000000000000000013f4ea83d0bd40e75c8222255bc855a974568dd400000000000000000000000013f4ea83d0bd40e75c8222255bc855a974568dd40000000000000000000000000000000000000000000000000b17542a687e000000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000380000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000022000000000000000000000000000000000000000000000000000000000000000840000000000000000000000002170ed0880ac9a755fd29b2688956bd959f933f800000000000000000000000013f4ea83d0bd40e75c8222255bc855a974568dd400000000000000000000000013f4ea83d0bd40e75c8222255bc855a974568dd400000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000104b858183f000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000800000000000000000000000004aab00833fd5f13d03115559277b0b8cc0a5ff360000000000000000000000000000000000000000000000000b17542a687e00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002b2170ed0880ac9a755fd29b2688956bd959f933f80001f4bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000240000000000000000000000002170ed0880ac9a755fd29b2688956bd959f933f8000000000000000000000000bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c000000000000000000000000bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c00000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000004449404b7c00000000000000000000000000000000000000000000000000000000000000000000000000000000000000004aab00833fd5f13d03115559277b0b8cc0a5ff36000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000005000000000000000000000000b971ef87ede563556b2ed4b1c0b0019111dd85d2000000000000000000000000b971ef87ede563556b2ed4b1c0b0019111dd85d200000000000000000000000000000000000000000000000002c5d50a9a1f800000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000380000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000022000000000000000000000000000000000000000000000000000000000000000840000000000000000000000002170ed0880ac9a755fd29b2688956bd959f933f8000000000000000000000000b971ef87ede563556b2ed4b1c0b0019111dd85d2000000000000000000000000b971ef87ede563556b2ed4b1c0b0019111dd85d200000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000104b858183f000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000800000000000000000000000004aab00833fd5f13d03115559277b0b8cc0a5ff3600000000000000000000000000000000000000000000000002c5d50a9a1f80000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002b2170ed0880ac9a755fd29b2688956bd959f933f80001f4bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000240000000000000000000000002170ed0880ac9a755fd29b2688956bd959f933f8000000000000000000000000bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c000000000000000000000000bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c00000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000004449404b7c00000000000000000000000000000000000000000000000000000000000000000000000000000000000000004aab00833fd5f13d03115559277b0b8cc0a5ff36000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
      "value": "0x0de0b6b3a7640000",
      "chainId": "1"
    }
  ]
}
```

2. fail to get swap transaction calldata

> 200 Response

```
{
    "errno": <error code>,
    "message": <detailed error message>
}
```

**Note**: error code can be found in [here](error-code-list.md)\