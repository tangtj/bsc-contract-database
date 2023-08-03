// SPDX-License-Identifier: MIT
pragma solidity >=0.7.8;
pragma experimental ABIEncoderV2;

interface TokenLike {
    function transferFrom(address,address,uint) external;
    function transfer(address,uint) external;
    function balanceOf(address) external view returns (uint);
    function decimals() external view returns (uint);
    function symbol() external view returns (string memory);
}
interface NFTLike {
    function discount(address) external view returns (uint);
}
contract LimitEntry {
    // --- Auth ---
    mapping (address => uint) public wards;
    function rely(address usr) external  auth {  wards[usr] = 1; }
    function deny(address usr) external  auth {  wards[usr] = 0; }
    modifier auth {
        require(wards[msg.sender] == 1, "LimitEntry/not-authorized");
        _;
    }
    // 发布的订单数据
    struct Limit {
        uint256 id;           //订单号
        address asses;        //卖出资产合约地址
        uint256 amount;       //卖出资产数量
        address pro;          //定价资产合约地址
        uint256 price;        //资产价格
        address seller;       //发布者地址  
    }
    struct LimitList {
        uint256 id;           //订单号
        string symbol;        //资产名称
        uint256 what;         //交易类型
        uint256 price;        //资产价格
        uint256 amount;       //剩余数量  
    }
    struct LimitSwap {
        uint256 id;           //成交订单号
        uint256 limitId;      //发布订单号
        uint256 inAmount;     //买入资产数量
        uint256 outAmount;     //支出资产数量
        address buyer;        //买入者地址  
    }
    struct SwapList {
        uint256 id;           //订单号
        uint256 limitId;      //发布订单号
        string  symbol;       //资产名称
        uint256 what;         //交易类型
        uint256 price;        //资产价格
        uint256 inAmount;     //买入资产数量
        uint256 outAmount;    //支出金额 
    }
    mapping (uint256 => Limit)                          public limitOrder;       //订单编号对应的订单
    mapping (uint256 => LimitSwap)                      public swapOrder;        //成交编号对应的订单
    uint256                                             public order;            //发布的总订单数量
    mapping (address => uint256)                        public issue;            //所有者发布的订单数
    mapping (address => uint256)                        public deal;             //所有者交易订单数
    mapping (uint256 => uint256)                        public dealList;         //订单成交的笔数
    mapping (address => uint256)                        public service;          //手续费
    NFTLike                                             public vip;              //会员等级合同地址
    bool                                                public startvip;         //启用VIP手续费打折
    bool                                                public SellFree;         //开启卖出方手续费
    bool                                                public BuyFree;          //开启买入方手续费
    uint256                                             public syscount = 10;         //
    uint256                                             public syslength = 100;        //
    uint256                                             public live = 1;             //系统激活标示
    address                                             public usdt = 0xE85131c9530A2Fc55D3587F914Ba6c1415f7EF86;          
    uint256                                             public rati = 2000;             //手续费比例
    string[]                                            public menu;
    mapping (string => address)                         public memuForString; 
    mapping (address => bool)                           public isMain;
    uint256                                             public swap;             //成交的总订单数量
    mapping (address => mapping (uint256 => uint256))   public ownerOrder;       //所有者发布的订单序号对应的订单
    mapping (address => mapping (uint256 => uint256))   public succeed;          //所有者成功交易的订单序号对应的订单
    mapping (uint256 => mapping (uint256 => uint256))   public succeedList;      //所有者成功交易的订单序号对应的订单

    event SellLimit( uint256  indexed  order, 
                     address  indexed  owner,
                     address  indexed  asses,
                     address           pro
                );
    event ModiPrice( uint256  indexed  order, 
                     uint256           unit
                );
    event ModiAmount( uint256  indexed  order,
                      uint256  indexed  amount
                );
    event Taker(uint256  indexed  limitOrder, 
                uint256  indexed  swapOrder,
                address           seller,
                address  indexed  buyer
                );
           
    // --- Init ---
    constructor()  {
        wards[msg.sender] = 1;
        isMain[usdt] = true;
    }
    // --- Math ---
    function add(uint x, int y) internal pure returns (uint z) {
        z = x + uint(y);
        require(y >= 0 || z <= x);
        require(y <= 0 || z >= x);
    }
    function sub(uint x, int y) internal pure returns (uint z) {
        z = x - uint(y);
        require(y <= 0 || z <= x);
        require(y >= 0 || z >= x);
    }
    function mul(uint x, int y) internal pure returns (int z) {
        z = int(x) * y;
        require(int(x) >= 0);
        require(y == 0 || z / y == int(x));
    }
    function add(uint x, uint y) internal pure returns (uint z) {
        require((z = x + y) >= x);
    }
    function sub(uint x, uint y) internal pure returns (uint z) {
        require((z = x - y) <= x);
    }
    function mul(uint x, uint y) internal pure returns (uint z) {
        require(y == 0 || (z = x * y) / y == x);
    }
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
          require(b != 0);
          return a % b;
    }
        function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }

     // 设置vip合约地址
    function setust(address ust) external auth {
        vip = NFTLike(ust);
    }
    function setMain(address ust) external auth {
        isMain[ust] = !isMain[ust];
     }       
    //  设置手续费
    function global(uint data) external  auth {
         rati = data;   //6小数位
    }
    // 升级合约时停止系统
    function cage(uint256 what) external  auth { live = what; }

    function setMenu(string[] memory symbols) external auth {
        menu = symbols;
    }
    function addMenu(address asset,string memory symbol) external auth {
        menu.push(symbol);
        memuForString[symbol] = asset;
    }
    function getMenu() external view returns(string[] memory) {
        return menu;
    }
    // 启动/停止 VIP
    function setBool(uint256 what,uint256 data) external auth {
        if (what == 1) startvip = !startvip;
        if (what == 2) SellFree = !SellFree;
        if (what == 3) BuyFree  = !BuyFree;
        if (what == 4) syscount  = data;
        if (what == 5) syslength  = data;
     }

    // 提取手续费
    function WithService(address pro,address usr,uint256 wad) external  auth {
        require(wad <= service[pro] , "LimitEntry/not-amount");
        TokenLike(pro).transfer(usr, wad);
    }
      // 发布订单 
    function sellLimit(address asses, uint256 wad, address _pro, uint256 _price,address owner) external  {
        require(live == 1 , "LimitEntry/not-live");
        uint256 frontAmount = TokenLike(asses).balanceOf(address(this));
        TokenLike(asses).transferFrom(msg.sender, address(this), wad);
        uint256 afterAmount = TokenLike(asses).balanceOf(address(this));
        uint256 inputAmount = sub(afterAmount,frontAmount);
        order += 1;
        Limit memory merc = limitOrder[order];
            merc.id = order; 
            merc.asses = asses;
            merc.amount = inputAmount;
            merc.pro = _pro;
            merc.price = _price;
            merc.seller = owner;
        limitOrder[order] = merc;
        issue[owner] +=1;
        ownerOrder[owner][issue[owner]] = order;
        emit SellLimit (order,owner,asses,_pro);
    }
    // 修改订单中的单价
    function modifyPrice(uint256 i, uint256 _price) external  {
         require(limitOrder[i].seller == msg.sender, "LimitEntry/not-mercher");
         limitOrder[i].price = _price;
         emit ModiPrice(i,_price);
    }
    
        // 取消/修改订单数量
    function modifyAmount(uint256 i,uint256 wad) external  {
         require(limitOrder[i].seller == msg.sender, "LimitEntry/not-mercher");
         address asses = limitOrder[i].asses;
         uint amount = limitOrder[i].amount;
         if(wad > amount){
             uint addAmount = wad - amount;
             uint256 frontAmount = TokenLike(asses).balanceOf(address(this));
             TokenLike(asses).transferFrom(msg.sender, address(this), addAmount);
             uint256 afterAmount = TokenLike(asses).balanceOf(address(this));
             uint256 inputAmount = sub(afterAmount,frontAmount);
             wad = add(amount,inputAmount);
         }
         else if (wad < amount) {
             uint decreaseAmount = amount - wad;
             TokenLike(asses).transfer(msg.sender, decreaseAmount);
         }
         limitOrder[i].amount = wad; 
         emit ModiAmount(i,wad);
    }
     // 接单
    function taker(uint256 i,uint256 wad,address owner) public returns (uint256) {
        require(live == 1, "LimitEntry/not-live");
        require(wad <= limitOrder[i].amount, "LimitEntry/not-amount");
        limitOrder[i].amount = sub(limitOrder[i].amount,wad);
        address asses = limitOrder[i].asses;
        address pro = limitOrder[i].pro;
        uint256 price = limitOrder[i].price;
        address seller = limitOrder[i].seller;
        uint256 proAmount = mul(wad,price)/10**TokenLike(asses).decimals();

        swap += 1;
        LimitSwap memory merc = swapOrder[swap];
            merc.id = swap; 
            merc.limitId = i;
            merc.inAmount = wad;
            merc.outAmount = proAmount;
            merc.buyer = owner;
        swapOrder[swap] = merc;
        deal[owner] += 1;
        succeed[owner][deal[owner]] = swap;
        dealList[i] += 1;
        succeedList[i][dealList[i]] = swap;

        if (SellFree) {
            uint256 free = mul(proAmount,rati)/10**6;
            if (startvip){
                uint dis = vip.discount(seller);
                free = mul(free,dis)/1000;
            }
            TokenLike(pro).transferFrom(msg.sender,address(this), free);
            service[pro] += free;
            proAmount = sub(proAmount,free);
        }
        TokenLike(pro).transferFrom(msg.sender,seller, proAmount);

        if (BuyFree) {
            uint256 free = mul(wad,rati)/10**6;
            if (startvip){
                uint disSell = vip.discount(owner);
                free = mul(free,disSell)/1000;
            }
            service[asses] += free;
            wad = sub(wad,free);
        }
        TokenLike(asses).transfer(owner, wad);

        emit Taker (swap,i,seller,owner);
        return swapOrder[swap].outAmount;
    } 
    function takeres(uint256[] memory orderes, uint256[] memory amountes,address owner) public {
        require( orderes.length == amountes.length, "LimitEntry/not-length");
        for (uint i = 0; i < orderes.length; ++i) {
            taker(orderes[i],amountes[i],owner);
        }
    }

    function fastBuy(address asses, uint256 outAmount , address pro,address owner) public returns (uint256){
        uint256 frontAmount = TokenLike(pro).balanceOf(msg.sender);
        while (outAmount > 0) {
            Limit[] memory result = sort(asses,pro,syslength,syscount);
            uint256 length = result.length;
            for (uint256 i = 0; i < length;i++) {
                if(result[i].amount >= outAmount) {
                    taker(result[i].id,outAmount,owner);
                    outAmount = 0;
                    i = length;
                }
                else {
                    taker(result[i].id,result[i].amount,owner);
                    outAmount = sub(outAmount,result[i].amount);
                }
            }
        }
        uint256 afterAmount = TokenLike(pro).balanceOf(msg.sender);
        uint256 inputAmount = sub(frontAmount,afterAmount);
        return inputAmount;
    }
    function fastBuySlippage(address asses, uint256 outAmount , address pro, uint256 maxInAmount,address owner) public {
        uint256 inAmount = fastBuy(asses, outAmount, pro,owner);
        require(inAmount <= maxInAmount, "LimitEntry/not-max");
    }
    function fastSell(address asses, uint256 inAmount , address pro,address owner) public returns (uint256){
        uint256 frontAmount = TokenLike(asses).balanceOf(owner);
        while (inAmount > 0) {
            Limit[] memory result = sort(asses,pro,syslength,syscount);
            uint256 length = result.length;
            for (uint256 i = 0; i < length;i++) {
                uint256 amount = result[i].amount;
                uint256 price = result[i].price;
                uint256 outAmount = div(mul(inAmount,10**TokenLike(pro).decimals()),price);
                if(amount >= outAmount) {
                    taker(result[i].id,outAmount,owner);
                    inAmount = 0;
                    i = length;
                }
                else {
                    taker(result[i].id,amount,owner);
                    uint256 proAmount = mul(amount,price)/10**TokenLike(asses).decimals();
                    inAmount = sub(inAmount,proAmount);
                }
            }
        }
        uint256 afterAmount = TokenLike(asses).balanceOf(owner);
        uint256 assesAmount = sub(afterAmount,frontAmount);
        return assesAmount;
    }

    function fastSellSlippage(address asses, uint256 inAmount , address pro, uint256 minOutAmount,address owner) public {
        uint256 outAmount = fastSell(asses, inAmount, pro,owner);
        require(outAmount >= minOutAmount, "LimitEntry/not-min");
    }

    function sort(address asses, address pro,uint256 length,uint256 count) public view returns (Limit[] memory) {
        Limit[] memory result = new Limit[](length);
        uint size;
        for (uint i = order; i >= 1; --i) {
            Limit memory m = limitOrder[i];
            if (m.asses == asses && m.pro == pro && m.amount > 0) {
                size +=1 ;
                for (uint j = 0; j < size; ++j) {
                    Limit memory n = result[j];
                    if (n.pro == address(0)) result[j] = m;
                    else if ( m.price < n.price) {
                            for (uint k = size - 1; k >= j; --k) {
                            result[k + 1] = result[k];
                            if (k==0) break;
                            }
                          result[j] = m;
                          break;
                        }
                    }
                }if (size==length) break;
            }
        if (size < count ) count = size;
        Limit[] memory resultes = new Limit[](count);   
        for (uint i = 0; i < count; ++i) {
            resultes[i] = result[i];
        }
        return resultes;
    }

    function ownerIssue(address usr, address asses,uint256 count,bool history) public view returns (LimitList[] memory result) {
        uint length = issue[usr];
        if (count < length) length = count;
        result = new LimitList[](length);
        uint max = issue[usr];
        uint j;
        for (uint i = max; i >=1 ; --i) {
            uint n = ownerOrder[usr][i];
            Limit memory m = limitOrder[n];
            LimitList memory mm;
            mm.id = m.id;
            address sift;
            if(isMain[m.asses]){
                mm.symbol = TokenLike(m.pro).symbol();
                mm.what = 1;
                mm.price = 1E36/m.price;
                sift = m.pro;
            }
            else{
                mm.symbol = TokenLike(m.asses).symbol();
                mm.what = 2;
                mm.price = m.price;
                sift = m.asses;
            }
            mm.amount = m.amount;
            if((asses == address(0) || asses == sift) && ((history && mm.amount == 0) || (!history && mm.amount >0))){
               result[j] = mm;
               j +=1;
               if(j==count) break;
            }
        }
        if(j<count) {
            LimitList[] memory resultSwapSum = new LimitList[](j);
            for (uint m = 0; m < j ; ++m) {
                 resultSwapSum[m] = result[m];
             }
             result = resultSwapSum;
        }
        return result;
    }
 
    //返回账户交易的订单列表 @count 最多返回的数量
     function ownerDeal(address usr, uint256 count) public view returns (LimitSwap[] memory) {
        uint length = deal[usr];
        if (count < length) length = count;
        LimitSwap[] memory result = new LimitSwap[](length);
        uint max = deal[usr];
        uint j;
        for (uint i = max; i >=1 ; --i) {
            uint n = succeed[usr][i];
            LimitSwap memory m = swapOrder[n];
            result[j] = m;
            j +=1;
            if (i-1 == max-length) break;
        }
        return result;
    }
     //返回账户交易的订单列表 @count 最多返回的数量
     function ownerDealForAll(address usr, address asses,uint256 count) public view returns (SwapList[] memory) {
        uint length = deal[usr];
        if (count < length) length = count;
        SwapList[] memory result = new SwapList[](length);
        uint max = deal[usr];
        uint j;
        for (uint i = max; i >=1 ; --i) {
            uint n = succeed[usr][i];
            LimitSwap memory nn = swapOrder[n];
            uint256 id = nn.limitId;
            SwapList memory mm;
            mm.id = nn.id;
            mm.limitId = nn.limitId;
            address sift;
            if(isMain[limitOrder[id].asses]){
                mm.symbol = TokenLike(limitOrder[id].pro).symbol();
                mm.what = 2;
                mm.inAmount = nn.outAmount;
                mm.outAmount = nn.inAmount;
                sift = limitOrder[id].pro;
            }
            else{
                mm.symbol = TokenLike(limitOrder[id].asses).symbol();
                mm.what = 1;
                mm.inAmount = nn.inAmount;
                mm.outAmount = nn.outAmount;
                sift = limitOrder[id].asses;
            }
            if(mm.inAmount !=0) mm.price = mm.outAmount*1E18/mm.inAmount;
            if(asses == address(0) || asses == sift){
               result[j] = mm;
               j +=1;
               if(j==count) break;
            }
        }
        if(j<count) {
            SwapList[] memory resultSwapSum = new SwapList[](j);
            for (uint m = 0; m < j ; ++m) {
                 resultSwapSum[m] = result[m];
             }
             result = resultSwapSum;
        }
        return result;
    }
    //返回限价单成交的交易列表 @count 最多返回的数量
     function limitDealListForId(uint256 i, uint256 count) public view returns (LimitSwap[] memory) {
        uint length = dealList[i];
        if (count < length) length = count;
        LimitSwap[] memory result = new LimitSwap[](length);
        uint max = dealList[i];
        uint j;
        for (uint k = max; k >=1 ; --k) {
            uint n = succeedList[i][k];
            LimitSwap memory m = swapOrder[n];
            result[j] = m;
            j +=1;
            if (k-1 == max-length) break;
        }
        return result;
    }
    function ownerIssue1(address usr, uint256 count) public view returns (Limit[] memory) {
        uint length = issue[usr];
        if (count < length) length = count;
        Limit[] memory result = new Limit[](length);
        uint max = issue[usr];
        uint j;
        for (uint i = max; i >=1 ; --i) {
            uint n = ownerOrder[usr][i];
            Limit memory m = limitOrder[n];
            result[j] = m;
            j +=1;
            if (i-1 == max-length) break;
        }
        return result;
    }

    //返回用户所有限价单成交的总交易列表
    function limitDealList(address usr,uint256 count,uint256 multiple) public view returns (SwapList[] memory) {
        Limit[] memory result = ownerIssue1(usr, count);
        uint length = result.length;
        SwapList[] memory resultSwap = new SwapList[](length*multiple);
        uint j;
        for (uint z = 0; z <length ; ++z) {
            uint256 i = result[z].id;
            LimitSwap[] memory resultSwapForId = limitDealListForId(i,count);
            uint lengthId =resultSwapForId.length;
            for (uint k = 0; k <lengthId ; ++k) {
                LimitSwap memory nn = resultSwapForId[k];
                SwapList memory mm;
                mm.id = nn.id;
                mm.limitId = nn.limitId;
                if(isMain[result[z].asses]){
                    mm.symbol = TokenLike(result[z].pro).symbol();
                    mm.what = 1;
                    mm.inAmount = nn.outAmount;
                    mm.outAmount = nn.inAmount;
                }
                else{
                    mm.symbol = TokenLike(result[z].asses).symbol();
                    mm.what = 2;
                    mm.inAmount = nn.inAmount;
                    mm.outAmount = nn.outAmount;
                }
                if(mm.inAmount !=0) mm.price = mm.outAmount*1E18/mm.inAmount;
                resultSwap[j] = mm;
                j +=1;
            }
        }
        SwapList[] memory resultSwapSum = new SwapList[](j);
        for (uint m = 0; m < j ; ++m) {
            resultSwapSum[m] = resultSwap[m];
        }
        return resultSwapSum;
    }
    function limitDealListFroUser(address usr,address asses,uint256 count,uint256 multiple) public view returns (SwapList[] memory) {
        SwapList[] memory result = limitDealList(usr,count,multiple);
        uint length = result.length;
        SwapList[] memory resultSwap = new SwapList[](length);
        uint j;
        for (uint z = 0; z <length ; ++z) {
            uint256 i = result[z].limitId;
            Limit memory limit = limitOrder[i];
            address sift;
            if(isMain[limit.asses]){
                sift = limit.pro;
            }
            else sift = limit.asses;
            if(asses == address(0) || asses == sift ){
               resultSwap[j] = result[z];
               j +=1;
            }
        }
        SwapList[] memory resultSwapSum = new SwapList[](j);
        for (uint m = 0; m < j ; ++m) {
            resultSwapSum[m] = resultSwap[m];
        }
        return resultSwapSum;
    }
    function fastBuyCall(address asses, uint256 outAmount , address pro) public view returns (uint256){
        uint256 inputAmount;
        while (outAmount > 0) {
            Limit[] memory result = sort(asses,pro,syslength,syscount);
            uint256 length = result.length;
            for (uint256 i = 0; i < length;i++) {
                if(result[i].amount >= outAmount) {
                    uint inAmount = mul(outAmount,result[i].price)/10**TokenLike(asses).decimals();
                    inputAmount += inAmount;
                    outAmount = 0;
                    i = length;
                }
                else {
                    uint inAmount = mul(result[i].amount,result[i].price)/10**TokenLike(asses).decimals();
                    inputAmount += inAmount;
                    outAmount = sub(outAmount,result[i].amount);
                }
            }
        }
        return inputAmount;
    }
    function fastSellCall(address asses, uint256 inAmount , address pro) public view returns (uint256){
        uint256 _outAmount;
        while (inAmount > 0) {
            Limit[] memory result = sort(asses,pro,syslength,syscount);
            uint256 length = result.length;
            for (uint256 i = 0; i < length;i++) {
                uint256 amount = result[i].amount;
                uint256 price = result[i].price;
                uint256 outAmount = div(mul(inAmount,10**TokenLike(pro).decimals()),price);
                if(amount >= outAmount) {
                    _outAmount +=outAmount;
                    inAmount = 0;
                    i = length;
                }
                else {
                    _outAmount += amount;
                    uint256 proAmount = mul(amount,price)/10**TokenLike(asses).decimals();
                    inAmount = sub(inAmount,proAmount);
                }
            }
        }
        return _outAmount;
    }
}