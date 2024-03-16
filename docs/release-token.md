# solana 创建 token

## 安装

- 科学上网
- Rust
- [Solana CLI](https://docs.solanalabs.com/cli/install)

以上条件具备后安装 `spl-token` 命令行工具

```shell
cargo install spl-token-cli
```

安装成功后，使用spl-token --help查看命令

## 配置

```shell
% solana config get 
Config File: /Users/walker/.config/solana/cli/config.yml
RPC URL: http://localhost:8899 
WebSocket URL: ws://localhost:8900/ (computed)
Keypair Path: /Users/walker/.config/solana/id.json 
Commitment: confirmed 
```
在测试网上发布，设置测试网的URL

```shell
solana config set --url https://api.testnet.solana.com
```
## 创建keypair：
```shell
solana-keygen new --force
```

## 获取测试token

```shell
% solana airdrop 1
Requesting airdrop of 1 SOL

Signature: 5MqBQ2XteAxLmWt7YN9J5J6mBrGEfEgDWpZfFeqro1W5ALBKK4fFiDTfCNuQpX5WKzGcoKMME6pFkjfogC4C71ZB

1 SOL
```

## 创建token
```shell
% spl-token create-token
Creating token 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG

Signature: 2y3LrMFhYVKTS43BfXgrnJL76GuAMcMwH62gMAjfxXznf3QifHKWGxjU8ErVfP99aPCaTXxMsH9SowfPADdRtzGd
```
token是：2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG
这时候还没有supply，我们需要mint一些币

```shell
##还没有供应supply
% spl-token supply 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG
0
##创建一个新的account

% spl-token create-account 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG
Creating account 3X27oitmLeSshkY3aH5C6bKVDcVYxftAqUv133tLw8ew

Signature: vdomF5LhKBDQgZ1mfbDaEog95sscHVQm81W28nQ4nU6QM1ifQAYwKFwwrDS8JsSy29NY2BWp8eyHp2fdm89v3MD

##查看2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG 余额 为0
% spl-token balance 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG
0
## mint 98765个token
% spl-token mint 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG 98765
Minting 98765 tokens
  Token: 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG
  Recipient: 3X27oitmLeSshkY3aH5C6bKVDcVYxftAqUv133tLw8ew

Signature: 4ywjPoWG4Rfn3DS4whFqyqUCQ1AJBPnKs5UtdFnmU43uZGqT2zQweq5gY2Yri85bH3woHssUvpJhkCwoJgK7KzZL

## 查询supply ，已经有98765个token了，成功
% spl-token supply 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG
98765
% spl-token balance 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG
98765
```
## 转账

需要使用一个新的keypair,然后create-account

```shell
% spl-token create-account -C /Users/walker/.config/solana/cli/config1.yml 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG
Creating account Gt653K6CA9db5TeJoCte3n8f56HP2PFpe7kT4bY7bA8Q

Signature: 5e2iNV8BBtbBgFtUBAxffHsmTMaVUtgKBDvVhjLLzygHGeG9LEm9p4hqzgfjhHEaKejtWCwuLunb7QnqmdWjTdUH
```
`Gt653K6CA9db5TeJoCte3n8f56HP2PFpe7kT4bY7bA8Q` 作为receiver

```shell
% spl-token  transfer --fund-recipient 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG 99 JCvUBQ3DEVj5R4i6b3XtMdK2iR65qtzAgavyKbDqX9fp
Transfer 99 tokens
  Sender: 3X27oitmLeSshkY3aH5C6bKVDcVYxftAqUv133tLw8ew
  Recipient: JCvUBQ3DEVj5R4i6b3XtMdK2iR65qtzAgavyKbDqX9fp
  Recipient associated token account: Gt653K6CA9db5TeJoCte3n8f56HP2PFpe7kT4bY7bA8Q

Signature: 5rpu87jA7CUyd1hzGCjQuVuJvMz44cv3zzj745yJGxa1SrGeQv96gzyzLE6caDScmkEqXmNSBZw9inPAXRjcqTVJ
```
## 查看交易详情

```shell
% solana confirm -v 5rpu87jA7CUyd1hzGCjQuVuJvMz44cv3zzj745yJGxa1SrGeQv96gzyzLE6caDScmkEqXmNSBZw9inPAXRjcqTVJ
RPC URL: https://api.testnet.solana.com
Default Signer Path: /Users/walker/.config/solana/id.json
Commitment: confirmed

Transaction executed in slot 97945230:
  Block Time: 2021-10-07T18:25:21+08:00
  Recent Blockhash: BXp1uFt3dgi4qVMcmLD7jasjha9HBBjiLGxdtsh61YV1
  Signature 0: 5rpu87jA7CUyd1hzGCjQuVuJvMz44cv3zzj745yJGxa1SrGeQv96gzyzLE6caDScmkEqXmNSBZw9inPAXRjcqTVJ
  Account 0: srw- 72teF7azaqMGs4CQJYKJFcLduiwsahEYHaVtjx7RD377 (fee payer)
  Account 1: -rw- 3X27oitmLeSshkY3aH5C6bKVDcVYxftAqUv133tLw8ew
  Account 2: -rw- Gt653K6CA9db5TeJoCte3n8f56HP2PFpe7kT4bY7bA8Q
  Account 3: -r-- 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG
  Account 4: -r-x TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA
  Instruction 0
    Program:   TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA (4)
    Account 0: 3X27oitmLeSshkY3aH5C6bKVDcVYxftAqUv133tLw8ew (1)
    Account 1: 2ALcwQW6i4JxFNZtbTjiyawZeH3tRADuDPz7k8Mpw6iG (3)
    Account 2: Gt653K6CA9db5TeJoCte3n8f56HP2PFpe7kT4bY7bA8Q (2)
    Account 3: 72teF7azaqMGs4CQJYKJFcLduiwsahEYHaVtjx7RD377 (0)
    Data: [12, 0, 30, 220, 12, 23, 0, 0, 0, 9]
  Status: Ok
    Fee: ◎0.000005
    Account 0 balance: ◎0.99645912 -> ◎0.99645412
    Account 1 balance: ◎0.00203928
    Account 2 balance: ◎0.00203928
    Account 3 balance: ◎0.0014616
    Account 4 balance: ◎1.08999168
  Log Messages:
    Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA invoke [1]
    Program log: Instruction: TransferChecked
    Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA consumed 3735 of 200000 compute units
    Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA success
```

## Links

- https://github.com/solana-labs/solana-program-library
- https://spl.solana.com/token