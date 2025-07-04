# Call a chain

### Interactive Guidance (Recommended)

Interact with a chain **using** Pop CLI's interactive guidance by simply entering:

```shell
pop call chain
```

You will be prompted to select a pallet, the dispatchable function and its required arguments, connection details for the chain, and the account to sign the transaction.

<figure><img src="../.gitbook/assets/callchain.gif" alt="pop call chain"><figcaption><p>pop call chain</p></figcaption></figure>

### Manual (non-interactive)

If you prefer not to use interactive prompts, you can call the chain by specifying all the required arguments directly:

```shell
pop call chain --pallet System --function remark --args "0x11" --url ws://localhost:9944/ --suri //Alice --skip-confirm
```

```
┌   Pop CLI : Call a chain
│
◇  Would you like to dispatch this function call with `Root` origin?
│  No 
│
⚙  pop call chain --pallet System --function remark --args "0x11" --url ws://localhost:9944/ --suri //Alice
│  
⚙  Encoded call data: 0x00000411
│  
◇  Do you want to submit the extrinsic?
│  Yes 
│
◇  Extrinsic submitted with hash: "0xadbfbb2f92b22a2c5d6542ba80e4111b5c60057f594a6d155341879c5a46f96e"
│
└  Call complete.
```

#### Additional Options

* To dispatch a call with Root origin when the chain's runtime includes `pallet-sudo`, you can wrap the call in a `sudo.sudo()` call by using the `--sudo` flag:

```shell
pop call chain --pallet System --function remark --args "0x11" --url ws://localhost:9944/ --suri //Alice --sudo
```

* If you already have the SCALE-encoded call data, and want to directly submit the extrinsic:

```shell
pop call chain --call 0x00000411 --url ws://localhost:9944/ --suri //Alice
```

```
┌   Pop CLI : Call a chain
│
⚙  Encoded call data: 0x00000411
│  
◇  Do you want to submit the extrinsic?
│  Yes 
│
◇  Extrinsic submitted successfully with hash: "0x60b10fa42fa7bb9e36460d199cef55b28b41dae3f9bb3326fc0e584009ce305b"
│
└  Call complete.
```

**Technical Support**

* [Polkadot Stack Exchange](https://polkadot.stackexchange.com/)
  * Create a question and tag it with "[`pop`](https://substrate.stackexchange.com/tags/pop/info)"
  * Share the StackExchange question in our [Pop Support Telegram channel](https://t.me/pop\_support)