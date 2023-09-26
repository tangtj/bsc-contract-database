pragma solidity ^0.8.7;
// SPDX-License-Identifier: MIT

contract Counter {
    /**
    * Public counter variable
    */
    uint public counter;

    /**
    * Use an interval in seconds and a timestamp to slow execution of Upkeep
    */
    uint public interval;
    uint public lastTimeStamp;
    bool public onOffButton;
    event Logger(string message, uint timestamp,uint blocknbr, uint curcount);
    
    address private _owner;
    modifier onlyOwner() {
    require(owner() == msg.sender, "Ownership Assertion: Caller of the function is not the owner.");
    _;
    }
    function owner() public view virtual returns (address) {
        return _owner;
    }

    constructor(uint updateInterval) {
      _owner = msg.sender;
      interval = updateInterval;
      lastTimeStamp = block.timestamp;
      onOffButton = true;
      counter = 0;
    }

    function setInterval(uint newInterval) onlyOwner external{
        interval = newInterval;
    }

    function setOnOffButton(bool newval) onlyOwner external{
        onOffButton = newval;
    }

    function eligible() public view returns (bool) {
      return (block.timestamp - lastTimeStamp) > interval;
    }

    function reset() onlyOwner external{
      counter = 0;
    }

    function checkUpkeep(bytes calldata checkData) external view returns (bool, bytes memory) {
        bool upkeepNeeded = false;
        if (onOffButton){
            upkeepNeeded = eligible();
        }
        return(upkeepNeeded, checkData);
    }

    function performUpkeep(bytes calldata performData) external {
        if (eligible() ) {
            lastTimeStamp = block.timestamp;
            counter = counter + 1;
            emit Logger("add 1", block.timestamp,block.number, counter);
            performData;
        }
    }
}