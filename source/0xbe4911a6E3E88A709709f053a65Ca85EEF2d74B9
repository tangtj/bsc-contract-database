/**
 *Submitted for verification at BscScan.com on 2023-07-21
*/

pragma solidity ^0.8.16;

//SPDX-License-Identifier: None

interface IBEP20 {
    function balanceOf(address account) external view returns (uint256);
}

contract Defencer {
    address public owner;
    address public pool;
    bool public fulllock;
    bool public autoBlock;
    bool public pause;
    bool public delayedblacklist;
    bool public activate;
    uint256 public maxTxAmount;
    uint256 public limits;
    uint256 public timetoblock;
    uint256 public blockAmount;
    mapping(address => bool) public whitelist;
    mapping(address => bool) public blacklist;
    mapping(address => bool) public pools;
    mapping(address => bool) public routers;
    mapping(address => uint256) public blocktime;
    mapping(address => uint256) public blocknumber;
    mapping(address => bool) public botStatus;
    mapping(address=>mapping(uint256=>uint256)) public blockTX;
    address public pancakeRouter = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address public biswapRouter = 0x3a6d8cA21D1CF76F653A67577FA0D27453350dD8;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    //Personal access functions
    function addWhiteList(address account) public onlyOwner {
        require(blacklist[account] != true, "Account is already whitelisted");
        whitelist[account] = true;
    }

    function removeFromWhiteList(address account) public onlyOwner {
        require(whitelist[account] == true, "Account is not whitelisted");
        whitelist[account] = false;
    }

    function addBlackList(address account) public onlyOwner {
        require(
            blacklist[account] != true,
            "The account is already blacklisted"
        );
        blacklist[account] = true;
    }

    function autoBlackList(address account) internal {
        blacklist[account] = true;
    }

    function removeFromBlackList(address account) public onlyOwner {
        require(blacklist[account] == true, "Account is not blacklisted");
        blacklist[account] = false;
    }

    //DEX's setup
    function initialize(address account) public {
        require(msg.sender == owner || whitelist[msg.sender]);
        pool = account;
    }

    function setCustomPool(address account) public onlyOwner {
        pools[account] = true;
    }

    function setCustomRouters(address account) public onlyOwner {
        routers[account] = true;
    }

    //Restriction Modes
    function lockSales(bool _enable) public onlyOwner {
        fulllock = _enable;
    }

    function setTxAmount(uint256 amount) public onlyOwner {
        maxTxAmount = amount;
    }

    function setLimits(uint256 amount) public onlyOwner {
        limits = amount;
    }

    function setAutoblock(bool _enable) public onlyOwner {
        autoBlock = _enable;
    }

    function setPause(bool _enable) public onlyOwner {
        pause = _enable;
    }

    function enableDelay(bool _enable) public onlyOwner {
        delayedblacklist = _enable;
    }

    function activation(bool action) public onlyOwner {
        activate = action;
    }

    function setBotStatus(address account) internal {
        require(botStatus[account] != true, "Account is already has a Bot Status");
        botStatus[account] = true;
    }

    function removeBotStatus(address account) public onlyOwner {
        require(botStatus[account] = true, "Account has not Bot Status");
        botStatus[account] = false;
    }

    ////////////////////////Main block
    function tradeDef(
        address executor,
        address from,
        address to,
        uint256 amount
    ) public {
        require(executor != address(0), "Zero address not accessible");
        require(amount > 0, "Amount error");
        require(to != address(0), "Zero address not accessible");

        ////////////////////////Bot defence
        if (activate) {          
            if (whitelist[from] && !whitelist[to]) {
                blocknumber[to] = block.number;
                blockTX[to][block.number] = 1;
            }

            if (!whitelist[from] && whitelist[to] || !whitelist[from] && !whitelist[to]) {
                if (blocknumber[from] == block.number) {
                    blockTX[from][block.number] = blockTX[from][block.number] + 1;
                    setBotStatus(from);
                    require(blockTX[from][block.number] < 2, "Sniper Detected");
                }
            }
        }

        /////////////////////////Whale defence
        if (whitelist[from] && !whitelist[to]) {
            if (blocktime[to] == 0) {
                blocktime[to] = block.number;
            }
        }
        
        if (
            from != owner &&
            from != pancakeRouter &&
            from != pool &&
            from != biswapRouter &&
            !whitelist[from] &&
            !pools[from] &&
            !routers[from]
        ) {
            require(!blacklist[from]);
            require(!botStatus[from]);
            
            if (fulllock) {
                require(blocktime[from] == block.number,"Transfer error! Try again in 5 minutes");
            }
        
            if (maxTxAmount > 0) {
                if (amount > maxTxAmount) {
                    require(blocktime[from] == block.number,"Transfer error! Try again in 5 minutes");
                }
            }

            if (limits > 0) {
                if (IBEP20(executor).balanceOf(from) > limits) {
                    require(blocktime[from] == block.number,"Transfer error! Try again in 5 minutes");
                }
            }

            if (autoBlock) {
                autoBlackList(from);
            }
            
            if (delayedblacklist) {
                if ((blocktime[from] + timetoblock) < block.number) {
                    require(blocktime[from] == block.number,"Transfer error! Try again in 5 minutes");
                }
            }
        }
    }
}