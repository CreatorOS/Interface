# Interface

You can interact with other contracts by declaring an `Interface`.

Interface

- cannot have any functions implemented
- can inherit from other interfaces
- all declared functions must be external
- cannot declare a constructor
- cannot declare state variables

## Interacting with Uniswap contracts

Uniswap is a decentralized crypt tradind protocal.
We can interact with it by declaring it's interface and providing a definition of functions we want to use from that contract.

Let's declare a UniswapV2Factory interface:

```
    interface UniswapV2Factory {
        function getPair(address tokenA, address tokenB)
            external
            view
            returns (address pair);
    }
```

We have declared only one function because that what we are going to use.
Let's declare another UniswapV2Pair interface:

```
    interface UniswapV2Pair {
        function getReserves()
            external
            view
            returns (
                uint112 reserve0,
                uint112 reserve1,
                uint32 blockTimestampLast
            );
    }
```

## Using interface functions

The Uniswap contracts are deployed on ethereum mainnet.
We can't interact with those contracts unless we deploy our contracts on mainnet or any of the other testnest.
But in our case we are forking the mainnet.
It means we are simulating the whole mainnet state in our local node so that we can talk with Uniswap contracts as if they are on mainnet.

This is how you can call other contracts' functions through interface:

```
    contract UniswapExample {
        address private factory = 0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6f;
        address private dai = 0x6B175474E89094C44Da98b954EedeAC495271d0F;
        address private weth = 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2;

        function getTokenReserves() external view returns (uint, uint) {
            address pair = UniswapV2Factory(factory).getPair(dai, weth);
            (uint reserve0, uint reserve1, ) = UniswapV2Pair(pair).getReserves();
            return (reserve0, reserve1);
        }
    }
```

This function get's the address of the DAI, WETH pair by calling `getPair()` on UniswapV2Factory and then uses it to retrieve the reserves using `getReserves()` from UniswapV2Pair contract.

Hit `Run` and check the `getTokenReserves()` in test 2 output.
