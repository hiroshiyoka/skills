# Foundry Conventions

## Project structure

```
contract-project/
├── src/              # contracts
├── test/             # Foundry tests (*.t.sol)
├── script/           # deploy scripts (*.s.sol)
├── lib/              # forge dependencies (git submodules)
└── foundry.toml
```

## Commands

```bash
forge build                     # compile
forge test                      # run tests
forge test -vvv                 # verbose output for debugging failures
forge test --match-test <name>  # run a specific test
forge coverage                  # test coverage report
```

## Deploy script pattern

```solidity
// script/Deploy.s.sol
import "forge-std/Script.sol";

contract DeployScript is Script {
    function run() external {
        uint256 deployerKey = vm.envUint("PRIVATE_KEY");
        vm.startBroadcast(deployerKey);

        // deploy contracts here

        vm.stopBroadcast();
    }
}
```

```bash
forge script script/Deploy.s.sol --rpc-url $BASE_RPC_URL --broadcast --verify
```

## Network config (`foundry.toml`)

```toml
[rpc_endpoints]
base = "${BASE_RPC_URL}"
base_sepolia = "${BASE_SEPOLIA_RPC_URL}"

[etherscan]
base = { key = "${BASESCAN_API_KEY}" }
```

Always deploy and test on Base Sepolia before mainnet. Never commit `.env` with real private keys or API keys — see the repo's `.gitignore`.

## Test naming convention

- `test_<Behavior>` — normal-path test.
- `test_RevertWhen_<Condition>` — expected revert test.
- `testFuzz_<Behavior>` — fuzz test.

This keeps test output scannable and makes it obvious at a glance which tests are checking failure paths vs. success paths.
