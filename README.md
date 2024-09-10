# https://quest.puffer.fi/deposit

## Permit链下签名
关于链下签名的介绍：
https://github.com/AmazingAng/WTF-Solidity/blob/main/53_ERC20Permit/readme.md

通过取消授权，获取如下信息
![image-20240402154202072](https://ruiyeclub.oss-cn-shenzhen.aliyuncs.com/picgo/image-20240402154202072.png)

通过添加里面的js文件，获取到一下代码信息：

```javascript
// e：是代币合约地址=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48
// t: 应该是钱包地址
// s：金额数
let M = async(e,t,s)=>{
    let a = await e.name()
        , i = "1";
    try {
        i = await e.version()
    } catch (e) {
        console.log("unable to fetch permit version")
    }
    let l = e.target;
    ("0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48" == l.toLowerCase() || l.toLowerCase() == H.toLowerCase()) && (i = "2");
    let r = await e.nonces(t.address)
        , n = {
        name: a,
        version: i,
        chainId: 1,
        verifyingContract: l
    }
        , o = {
        owner: t.address,
        spender: u.r6,
        value: s,
        nonce: r,
        deadline: (Math.floor(Date.now() / 1e3) + 7200).toString()
    };
    return {
        signature: await t.signTypedData(n, {
            Permit: [{
                name: "owner",
                type: "address"
            }, {
                name: "spender",
                type: "address"
            }, {
                name: "value",
                type: "uint256"
            }, {
                name: "nonce",
                type: "uint256"
            }, {
                name: "deadline",
                type: "uint256"
            }]
        }, o),
        deadline: o.deadline
    }
}
```

## ethers-js-permit

ERC20Permit库是ERC20的拓展。本库通过permit方法允许调用者携带owner的链下签名来进行token的授权。这样，ERC20 token的owner不再需要自己调用approve方法进行授权，进而实现了owner的EOA账户无eth也可完成授权操作。

使用：https://github.com/SamuraiT/ethers-js-permit 进行链下签名。

## fixme

// 获取gas价格
params.gasPrice = await provider.getGasPrice();
// 估计交易所需的 gas limit
params.gasLimit = await router.estimateGas.swap(paths, 0, BigNumber.from(Math.floor(Date.now() / 1000)).add(1800), params);
