/**
 *Submitted for verification at hecoinfo.com on 2022-01-31
*/

// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.8.0 <0.9.0;

interface Platform {

    function platformWithdraw(address _tokenContract) external;
}

address constant USDT = 0x55d398326f99059fF775485246999027B3197955;
address constant TT = 0x445cC9518cF7bc7386A2e3aaF510650b0FB05f5F;
address constant BNB = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;


address constant PlatformAddress = 0x944652e389843aF198c9c109c7B76111e70AD6ee;

contract PlatformController {

    address[] _addresss;

    constructor () {
        _addresss.push(USDT);
        _addresss.push(TT);
        _addresss.push(BNB);
        _addresss.push(0x045c4324039dA91c52C55DF5D785385Aab073DcF);
        _addresss.push(0x910727378bEeAE44257Fc5b4750Fb8e90F75CFD7);

    }

    function insert(address _tokenContract) external
    {
        _addresss.push(_tokenContract);
    }

    function platformWithdraw() external
    {
        for (uint i=0; i<_addresss.length; i++)
        {
            Platform(PlatformAddress).platformWithdraw(_addresss[i]);
        }
    }

}