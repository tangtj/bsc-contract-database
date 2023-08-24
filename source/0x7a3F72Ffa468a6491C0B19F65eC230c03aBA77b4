//  _______ _           ___   ___ ___   ___                                        
// |__   __| |         / _ \ / _ \__ \ / _ \                                       
//    | |  | |__   ___| (_) | | | | ) | | | | ___  _ __ __ _                       
//    | |  | '_ \ / _ \> _ <| | | |/ /| | | |/ _ \| '__/ _` |                      
//    | |  | | | |  __/ (_) | |_| / /_| |_| | (_) | | | (_| |                      
//  __|_|  |_| |_|\___|\___/ \___/____|\___(_)___/|_|_ \__, |    _                 
// |  __ \                               | |    / ____| __/ |   | |                
// | |__) |_ _ _   _ _ __ ___   ___ _ __ | |_  | (___  |___/ ___| |_ ___ _ __ ___  
// |  ___/ _` | | | | '_ ` _ \ / _ \ '_ \| __|  \___ \| | | / __| __/ _ \ '_ ` _ \ 
// | |  | (_| | |_| | | | | | |  __/ | | | |_   ____) | |_| \__ \ ||  __/ | | | | |
// |_|   \__,_|\__, |_| |_| |_|\___|_| |_|\__| |_____/ \__, |___/\__\___|_| |_| |_|
//              __/ |                                   __/ |                      
//             |___/                                   |___/                       
// Payment system designed by Shahzain Tariq and Rico Cunningham for the betterment
// of the GSG Ecosystem.  Learn more about us @ www.the8020.org

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

interface IGS50 {
    function buy(address _referredAdd) external payable returns (uint256);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);
}

interface INFTRewardPool {
    function receiveEth() external payable;
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract MasterPaymentContract is Ownable {
    INFTRewardPool public communityPool;
    INFTRewardPool public accessPool;
    IGS50 public gs50Token;

    mapping(uint256 => AppSharesData) public getData;

    event GS50Bought(address indexed _user,uint256 _ethAmount, uint256 _id);

    struct AppSharesData {
        address[] addresses;
        uint256[] shares;
        bool sale;
    }

    constructor(
        address gs50TokenAddress,
        address accessPoolAdd,
        address communityPoolAdd
    ) {
        gs50Token = IGS50(gs50TokenAddress);
        accessPool = INFTRewardPool(accessPoolAdd);
        communityPool = INFTRewardPool(communityPoolAdd);
    }

    function buyGS50(uint256 id) external payable {
        require(getData[id].sale, "ERROR: sale not started");
        uint256 toBuyGS50 = (msg.value * 95) / 100;
        uint256 toSendToAccessPool = (msg.value * 2) / 100;
        uint256 toSendToCommPool = (msg.value * 3) / 100;

        gs50Token.buy{value: toBuyGS50}(_msgSender());
        accessPool.receiveEth{value: toSendToAccessPool}();
        communityPool.receiveEth{value: toSendToCommPool}();

        distributeGS50(id);

        emit GS50Bought(_msgSender(),msg.value,id);
    }


    function getApplicationShareData(
        uint256 id
    ) public view returns (AppSharesData memory) {
        return (getData[id]);
    }

    function setCommunityRewardPool(address add) external onlyOwner {
        communityPool = INFTRewardPool(add);
    }

     function setAccessRewardPool(address add) external onlyOwner {
        accessPool = INFTRewardPool(add);
    }

    function set8020(address add) external onlyOwner {
        gs50Token = IGS50(add);
    }


    function setApplicationShareData(
        address[] memory adds,
        uint[] memory _shares,
        bool _sale,
        uint256 id
    ) external onlyOwner {
        require(adds.length > 0 && adds.length < 6,"ERROR: address limit error");
        require(adds.length == _shares.length,"ERROR: addresses doesn't match shares");
        AppSharesData memory AppSharesDataLocal = AppSharesData(
            adds,
            _shares,
            _sale
        );
       getData[id] = AppSharesDataLocal;
    }

    function distributeGS50(uint256 id) internal {
        uint256 tokenBalance = gs50Token.balanceOf(address(this));
        uint256 userShareBack = tokenBalance*2/100;
        tokenBalance -= userShareBack;
        gs50Token.transfer(_msgSender(),userShareBack);

        for(uint256 i=0; i< getData[id].addresses.length; i++){
            gs50Token.transfer(getData[id].addresses[i],tokenBalance*getData[id].shares[i]/100);
        }
    }
}