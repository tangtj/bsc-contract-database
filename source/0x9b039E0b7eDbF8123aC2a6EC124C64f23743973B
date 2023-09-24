// SPDX-License-Identifier: UNLISCENSED

pragma solidity 0.8.4;


contract FuelTokenContract {
    string public name = "Fuel Token";
    string public symbol = "FUEL";
    uint256 public totalSupply = 500000000000000000000000;
    mapping(uint => uint256) public icoSupplyMap;
    mapping(uint => uint) public rates;
    uint256 public icoSupply = 50000 ether;
    uint256 public idoSupply = 75000 ether;
    uint8 public decimals = 18;
    uint public idoUId = 0;
    uint public icoUId = 0;
    uint public idoValue = 1000 ether;
    uint public icoPhase = 1;
    bool public icoCompleted = false;
    address public owner;
    address public idoAdmin;
    uint public transFees = 5;
    uint public idoRate = 0;
    uint public balUpdCount = 1;
    
    struct IDOUser {
        bool exist;
        uint id;
        uint rate;
        uint cTime;
    }
    
    struct ICOUser {
        bool exist;
        uint id;
        uint amt;
        uint cTime;
        uint frstMnth;
        uint scndMnth;
        uint thrdMnth;
        uint frthMnth;
        uint amtSent;
        bool icoFlag;
    }
    
    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner,address indexed _spender,uint256 _value);
    event IDOBuy(address indexed _user,uint256 _value,uint time);
    event ICOBuy(address indexed _user,uint256 _value,uint time,uint rate);
    event IDOBonusTransfer(address indexed _from, address indexed _to, uint256 _value,uint time);

    mapping(address => uint256) public balanceOf;
    mapping(address => IDOUser) public idoUsers;
    mapping(uint => address) public idToAddress;
    mapping(address => ICOUser) public icoUsers;
    mapping(uint => address) public icoIdToAddress;
    mapping(address => mapping(address => uint256)) public allowed;

    modifier onlyOwner(){
        require(msg.sender==owner,"only owner allowed");
        _;
    }
    
    function setIdoRate(uint rate) public onlyOwner returns(bool){
        idoRate = rate;
        return true;
    }
    
    function setIdoAdminAddr(address _addr) public onlyOwner returns(bool){
        idoAdmin = _addr;
        return true;
    }

    constructor(address idoAdm) {
        owner = msg.sender;
        idoAdmin = idoAdm;
        balanceOf[msg.sender] = totalSupply - icoSupply - idoSupply;
       
        rates[1] = 2000;
        rates[2] = 1000;
        rates[3] = 800;
        rates[4] = 500;
        rates[5] = 400;
    }
    
    function buyIDO(uint rate) public payable returns (bool) {
        require(idoSupply >= idoValue,"IDO not available");
        require(!idoUsers[msg.sender].exist,"you can buy IDO only one time");
        if(rate <= 0){
            rate = idoRate;
        }
        require(msg.value*rate <= idoValue,"Sent value is not correct");
        
        idoUId++;
        IDOUser memory uObj = IDOUser(true,idoUId,rate,block.timestamp);
        idoUsers[msg.sender] = uObj;
        idToAddress[idoUId] = msg.sender;
        balanceOf[msg.sender] += idoValue;
        idoSupply -= idoValue;
        emit IDOBuy(msg.sender,idoValue,block.timestamp);
        payable(idoAdmin).transfer(msg.value);
        return true;
    }
    
    function buyICO() public payable returns (bool) {
        require(!icoCompleted,"ICO completed");
        uint256 _value = 0;
        uint256 rate = rates[icoPhase];
        if(icoSupplyMap[icoPhase] > 0){
            _value = msg.value * rate;
            if(_value > icoSupplyMap[icoPhase]){
                revert("bnb sent not correct");
            }else{
                icoSupply -= _value;
                icoSupplyMap[icoPhase] -= _value;
            }
        }
        if(icoSupplyMap[icoPhase] == 0){
            icoPhase++;
            if(icoPhase==6){
                icoCompleted = true;
            }
        }
        
        if(icoUsers[msg.sender].exist){
            uint icoAmt = icoUsers[msg.sender].amt + _value;
            icoUsers[msg.sender].amt = icoAmt;
            icoUsers[msg.sender].cTime = block.timestamp;
            icoUsers[msg.sender].frstMnth = icoAmt*10/100;
            icoUsers[msg.sender].scndMnth = icoAmt*30/100;
            icoUsers[msg.sender].thrdMnth = icoAmt*60/100;
            icoUsers[msg.sender].frthMnth = icoAmt;
        }else{
            icoUId++;
            icoUsers[msg.sender].exist = true;
            icoUsers[msg.sender].id = icoUId;
            icoUsers[msg.sender].amt = _value;
            icoUsers[msg.sender].cTime = block.timestamp;
            icoUsers[msg.sender].frstMnth = _value*10/100;
            icoUsers[msg.sender].scndMnth = _value*30/100;
            icoUsers[msg.sender].thrdMnth = _value*60/100;
            icoUsers[msg.sender].frthMnth = _value;
            icoUsers[msg.sender].amtSent = 0;
            icoUsers[msg.sender].icoFlag = true;
            icoIdToAddress[icoUId] = msg.sender;    
        }
        
        balanceOf[msg.sender] += _value;
        emit ICOBuy(msg.sender,_value,block.timestamp,rate);
        return true;
    }
    
    function saveOldIcoData(address _addr,uint pk,uint amt,uint ctime,uint fMnth,uint sMnth,uint tMnth,uint frMnth,uint amtSent) public onlyOwner returns(bool) {
        icoUsers[_addr].exist = true;
        icoUsers[_addr].id = pk;
        icoUsers[_addr].amt = amt;
        icoUsers[_addr].cTime = ctime;
        icoUsers[_addr].frstMnth = fMnth;
        icoUsers[_addr].scndMnth = sMnth;
        icoUsers[_addr].thrdMnth = tMnth;
        icoUsers[_addr].frthMnth = frMnth;
        icoUsers[_addr].amtSent = amtSent;
        icoUsers[_addr].icoFlag = true;
        icoIdToAddress[pk] = _addr;    
        
        emit ICOBuy(_addr,amt,ctime,rates[1]);
        return true;
    }
    
    function saveOldIdoData(address _addr,uint pk,uint ctime) public onlyOwner returns(bool){
        IDOUser memory uObj = IDOUser(true,pk,300 ether,ctime);
        idoUsers[_addr] = uObj;
        idToAddress[pk] = _addr;
        emit IDOBuy(_addr,idoValue,ctime);
        return true;
    }
    
    function saveTknConstants(uint icoS,uint idoS,uint icoCId,uint idoCId,uint icoPhs) public onlyOwner returns(bool){
        icoSupply = icoS;
        idoSupply = idoS;
        icoUId = icoCId;
        idoUId = idoCId;
        icoPhase = icoPhs;
        return true;
    }
    
    function saveIcoSupplyMapVal(uint[] memory mapVals) public onlyOwner returns(bool){
        for(uint i=0;i<mapVals.length;i++){
            icoSupplyMap[i+1] = mapVals[i];
        }
        return true;
    }
    
    function saveBalanceForUser(address _addr,uint balVal) public onlyOwner returns(bool){
        if(balUpdCount<34){
            balanceOf[_addr] = balVal;
            balUpdCount++;
        }
        
        return true;
    }
    
    function allowance(address allow_owner, address delegate) public view returns (uint) {
        return allowed[allow_owner][delegate];
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(msg.sender==owner || icoCompleted,"wait for ico completion");
        require(msg.sender==owner || idoSupply==0,"wait for ido completion");
        require(balanceOf[msg.sender] >= _value);
        if(idoUsers[msg.sender].exist){
            if((balanceOf[msg.sender] - _value) < (1000 ether) && block.timestamp < (idoUsers[msg.sender].cTime + 15778458)){
                revert("your funds are locked for while");
            }
        }
        
        if(icoUsers[msg.sender].exist && (icoUsers[msg.sender].amt != icoUsers[msg.sender].amtSent)){
            uint ttlCanSend = 0;
            if(block.timestamp < (icoUsers[msg.sender].cTime + 2629743)){
                ttlCanSend = icoUsers[msg.sender].frthMnth - icoUsers[msg.sender].amtSent;
            }else if(block.timestamp < (icoUsers[msg.sender].cTime + 5259486) && block.timestamp >= (icoUsers[msg.sender].cTime + 2629743)) {
                ttlCanSend = icoUsers[msg.sender].scndMnth - icoUsers[msg.sender].amtSent;
            }else if(block.timestamp < (icoUsers[msg.sender].cTime + 7889229) && block.timestamp >= (icoUsers[msg.sender].cTime + 5259486)) {
                ttlCanSend = icoUsers[msg.sender].thrdMnth - icoUsers[msg.sender].amtSent;
            }else if(block.timestamp >= (icoUsers[msg.sender].cTime + 7889229)){
                icoUsers[msg.sender].icoFlag = false;
            }
            if(icoUsers[msg.sender].icoFlag && _value > ttlCanSend) {
                revert("due to ico locking you can not withdraw this amount");
            }
            if(icoUsers[msg.sender].icoFlag){
                icoUsers[msg.sender].amtSent += _value;    
            }else{
                icoUsers[msg.sender].amtSent = icoUsers[msg.sender].amt;
            }
            
        }
        
        balanceOf[msg.sender] -= _value;
        if(idoUId>0){
            uint256 fees = (_value * transFees)/100;
            uint256 eachAmt = fees/idoUId;
            for(uint i=1;i<=idoUId;i++){
                balanceOf[idToAddress[i]] += eachAmt;
                emit IDOBonusTransfer(msg.sender,idToAddress[i],eachAmt,block.timestamp);
            }
            balanceOf[_to] += (_value - fees);
            emit Transfer(msg.sender, _to, (_value - fees));
        }else{
            balanceOf[_to] += _value;
            emit Transfer(msg.sender, _to, _value);
        }
        
        return true;
    }
    
    function transferBNB(address _to, uint256 _value) public onlyOwner returns (bool success) {
        require(_value<=address(this).balance,"balance is not sufficient");
        payable(_to).transfer(_value);
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success)
    {
        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from,address _to,uint256 _value) public returns (bool success) {
        require(_value <= allowed[_from][msg.sender]);
        require(_from==owner || icoCompleted,"wait for ico completion");
        require(_from==owner || idoSupply==0,"wait for ido completion");
        require(balanceOf[_from] >= _value);
        if(idoUsers[_from].exist){
            if((balanceOf[_from] - _value) < (1000 ether) && block.timestamp < (idoUsers[_from].cTime + 15778458)){
                revert("your funds are locked for while");
            }
        }
        
        if(icoUsers[_from].exist && (icoUsers[_from].amt != icoUsers[_from].amtSent)){
            uint ttlCanSend = 0;
            if(block.timestamp < (icoUsers[_from].cTime + 2629743)){
                ttlCanSend = icoUsers[_from].frthMnth - icoUsers[_from].amtSent;
            }else if(block.timestamp < (icoUsers[_from].cTime + 5259486) && block.timestamp >= (icoUsers[_from].cTime + 2629743)) {
                ttlCanSend = icoUsers[_from].scndMnth - icoUsers[_from].amtSent;
            }else if(block.timestamp < (icoUsers[_from].cTime + 7889229) && block.timestamp >= (icoUsers[_from].cTime + 5259486)) {
                ttlCanSend = icoUsers[_from].thrdMnth - icoUsers[_from].amtSent;
            }else if(block.timestamp >= (icoUsers[_from].cTime + 7889229)){
                icoUsers[_from].icoFlag = false;
            }
            if(icoUsers[_from].icoFlag && _value > ttlCanSend) {
                revert("due to ico locking you can not withdraw this amount");
            }
            if(icoUsers[_from].icoFlag){
                icoUsers[_from].amtSent += _value;    
            }else{
                icoUsers[_from].amtSent = icoUsers[_from].amt;
            }
            
        }
        
        balanceOf[_from] -= _value;
        if(idoUId>0){
            uint256 fees = (_value * transFees)/100;
            uint256 eachAmt = fees/idoUId;
            for(uint i=1;i<=idoUId;i++){
                balanceOf[idToAddress[i]] += eachAmt;
                emit IDOBonusTransfer(_from,idToAddress[i],eachAmt,block.timestamp);
            }
            balanceOf[_to] += (_value - fees);
            allowed[_from][_from] -= _value;
            emit Transfer(_from, _to, (_value - fees));
        }else{
            balanceOf[_to] += _value;
            allowed[_from][_from] -= _value;
            emit Transfer(_from, _to, _value);
        }
        return true;
    }
}