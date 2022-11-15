
# zkdid

 ```mermaid
graph TD
  Creditial[(Creditial)]
  zkCreditial[/zkCreditial/]
  Circuit1{Circuit1}
  Circuit2{Circuit2}
  Circuit3{Circuit3}
  zkProof1[/zkProof1/]
  zkProof2[/zkProof2/]
  zkProof3[/zkProof3/]
  

  Creditial-->|CreateZkCreditial|zkCreditial
  Circuit1-..->zkProof1
  zkCreditial-..->zkProof1
  zkCreditial-..->zkProof2
  zkCreditial-..->zkProof3
  Circuit2-..->zkProof2
  Circuit3-..->zkProof3
  ```
  
   * **Creditial**: 记录证书明文, `issuer` 对证书进行签名(签名: `合约地址` `用户地址` `链ID?` `时间` `证书内容`) 达到不可篡改;
   * **zkCreditial**:   `issuer` 将 **Creditial** 经过 `AES` 加密后得到的`密文`(`issuer`可以解密,从而获得证书铭文) `issuer` 对证书进行签名(签名: `合约地址` `用户地址` `链ID?` `时间` `AES加密后的密文`) 达到不可篡改 和可认证;
   * **Circuit**: `issuer` 通过 **Creditial** 内的字段创建,用于生成对应的 **zkProof**
   * **zkProof** 是一个明文的 **Creditial**, 比如明文内容是 `来自USA, xx分数大于80, xx分数大于60, 已验证discord帐号, twitter帐号` 等;(同 **Creditial** 不可篡改,可认证.)

## ??

 - **zkCreditial** 是否需要上链? 好处(用户不需要下载保存证书,网站也不需要帮用户保存证书), 坏处(gas);
 - 合约上是否需要设计黑名单功能, 比如吊销执照等.
 - 合约应该设计为可升级;
 
