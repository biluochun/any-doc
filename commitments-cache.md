# commitments-cache

通过 `中心化服务` 和 `对称加密` 技术 缓存 `commitments`

```ts
  // 一个中心化接口服务
  const api = createServerApi();

  // 对称加密技术, 比如 AES
  const aes = createAES({ mode: 'cbc' });

  // 用户 UTXO 的私钥
  const UTXO_private_key = 'xxxxxxxxxx';

  // 签名出一个密码,或者直接用私钥作为密码
  const password = hash(UTXO_private_key);

  // 当用户 生产 或 查询 出 `commitments`, 缓存下来.
  {
    const commitments = 'a new commitments';

    // 这里用私钥作为密码将内容加密, 实际可以用私钥产生的签名.
    const secret_commitments = aes.encrypt({
      data: commitments,
      password: password
    });

    // 加密后得到的 secret_commitments 很安全,可以放心交给中心化服务缓存
    api.push({
      auth: sign('对当前时间 + 用途类型(存储secret_commitments)进行签名', UTXO_private_key),
      from: getPublickey(UTXO_private_key), // 以用户的公钥或者签名作为key
      data: secret_commitments
    });
  }

  // 用户可以通过自己的公钥或者签名获取 secret_commitments list.
  const secret_commitments_list = await api.get_secret_commitments({
    auth: sign('对当前时间 + 用途类型(获取secret_commitments)进行签名', UTXO_private_key),
    from: getPublickey(UTXO_private_key), // 以用户的公钥或者签名作为key
    pageSize: 100,
    page: 1
  });

  // 揭秘
  const commitments_list = secret_commitments_list.map(sc => {
    return aes.decrypt({
      data: sc,
      password: password
    });
  })


```