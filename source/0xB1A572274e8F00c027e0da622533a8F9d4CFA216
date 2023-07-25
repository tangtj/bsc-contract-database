// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;

interface AutomationCompatibleInterface {
  function checkUpkeep(bytes calldata checkData) external returns (bool upkeepNeeded, bytes memory performData);
  function performUpkeep(bytes calldata performData) external;
}

/**
 * @title Elliptic Curve Library
 * @dev Library providing arithmetic operations over elliptic curves.
 * This library does not check whether the inserted points belong to the curve
 * `isOnCurve` function should be used by the library user to check the aforementioned statement.
 * @author Witnet Foundation
 */
library EllipticCurve {

  // Pre-computed constant for 2 ** 255
  uint256 constant private U255_MAX_PLUS_1 = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

  /// @dev Modular euclidean inverse of a number (mod p).
  /// @param _x The number
  /// @param _pp The modulus
  /// @return q such that x*q = 1 (mod _pp)
  function invMod(uint256 _x, uint256 _pp) internal pure returns (uint256) {
    require(_x != 0 && _x != _pp && _pp != 0, "Invalid number");
    uint256 q = 0;
    uint256 newT = 1;
    uint256 r = _pp;
    uint256 t;
    while (_x != 0) {
      t = r / _x;
      (q, newT) = (newT, addmod(q, (_pp - mulmod(t, newT, _pp)), _pp));
      (r, _x) = (_x, r - t * _x);
    }

    return q;
  }

  /// @dev Modular exponentiation, b^e % _pp.
  /// Source: https://github.com/androlo/standard-contracts/blob/master/contracts/src/crypto/ECCMath.sol
  /// @param _base base
  /// @param _exp exponent
  /// @param _pp modulus
  /// @return r such that r = b**e (mod _pp)
  function expMod(uint256 _base, uint256 _exp, uint256 _pp) internal pure returns (uint256) {
    require(_pp!=0, "Modulus is zero");

    if (_base == 0)
      return 0;
    if (_exp == 0)
      return 1;

    uint256 r = 1;
    uint256 bit = U255_MAX_PLUS_1;
    assembly {
      for { } gt(bit, 0) { }{
        r := mulmod(mulmod(r, r, _pp), exp(_base, iszero(iszero(and(_exp, bit)))), _pp)
        r := mulmod(mulmod(r, r, _pp), exp(_base, iszero(iszero(and(_exp, div(bit, 2))))), _pp)
        r := mulmod(mulmod(r, r, _pp), exp(_base, iszero(iszero(and(_exp, div(bit, 4))))), _pp)
        r := mulmod(mulmod(r, r, _pp), exp(_base, iszero(iszero(and(_exp, div(bit, 8))))), _pp)
        bit := div(bit, 16)
      }
    }

    return r;
  }

  /// @dev Converts a point (x, y, z) expressed in Jacobian coordinates to affine coordinates (x', y', 1).
  /// @param _x coordinate x
  /// @param _y coordinate y
  /// @param _z coordinate z
  /// @param _pp the modulus
  /// @return (x', y') affine coordinates
  function toAffine(
    uint256 _x,
    uint256 _y,
    uint256 _z,
    uint256 _pp)
  internal pure returns (uint256, uint256)
  {
    uint256 zInv = invMod(_z, _pp);
    uint256 zInv2 = mulmod(zInv, zInv, _pp);
    uint256 x2 = mulmod(_x, zInv2, _pp);
    uint256 y2 = mulmod(_y, mulmod(zInv, zInv2, _pp), _pp);

    return (x2, y2);
  }

  /// @dev Derives the y coordinate from a compressed-format point x [[SEC-1]](https://www.secg.org/SEC1-Ver-1.0.pdf).
  /// @param _prefix parity byte (0x02 even, 0x03 odd)
  /// @param _x coordinate x
  /// @param _aa constant of curve
  /// @param _bb constant of curve
  /// @param _pp the modulus
  /// @return y coordinate y
  function deriveY(
    uint8 _prefix,
    uint256 _x,
    uint256 _aa,
    uint256 _bb,
    uint256 _pp)
  internal pure returns (uint256)
  {
    require(_prefix == 0x02 || _prefix == 0x03, "Invalid compressed EC point prefix");

    // x^3 + ax + b
    uint256 y2 = addmod(mulmod(_x, mulmod(_x, _x, _pp), _pp), addmod(mulmod(_x, _aa, _pp), _bb, _pp), _pp);
    y2 = expMod(y2, (_pp + 1) / 4, _pp);
    // uint256 cmp = yBit ^ y_ & 1;
    uint256 y = (y2 + _prefix) % 2 == 0 ? y2 : _pp - y2;

    return y;
  }

  /// @dev Check whether point (x,y) is on curve defined by a, b, and _pp.
  /// @param _x coordinate x of P1
  /// @param _y coordinate y of P1
  /// @param _aa constant of curve
  /// @param _bb constant of curve
  /// @param _pp the modulus
  /// @return true if x,y in the curve, false else
  function isOnCurve(
    uint _x,
    uint _y,
    uint _aa,
    uint _bb,
    uint _pp)
  internal pure returns (bool)
  {
    if (0 == _x || _x >= _pp || 0 == _y || _y >= _pp) {
      return false;
    }
    // y^2
    uint lhs = mulmod(_y, _y, _pp);
    // x^3
    uint rhs = mulmod(mulmod(_x, _x, _pp), _x, _pp);
    if (_aa != 0) {
      // x^3 + a*x
      rhs = addmod(rhs, mulmod(_x, _aa, _pp), _pp);
    }
    if (_bb != 0) {
      // x^3 + a*x + b
      rhs = addmod(rhs, _bb, _pp);
    }

    return lhs == rhs;
  }

  /// @dev Calculate inverse (x, -y) of point (x, y).
  /// @param _x coordinate x of P1
  /// @param _y coordinate y of P1
  /// @param _pp the modulus
  /// @return (x, -y)
  function ecInv(
    uint256 _x,
    uint256 _y,
    uint256 _pp)
  internal pure returns (uint256, uint256)
  {
    return (_x, (_pp - _y) % _pp);
  }

  /// @dev Add two points (x1, y1) and (x2, y2) in affine coordinates.
  /// @param _x1 coordinate x of P1
  /// @param _y1 coordinate y of P1
  /// @param _x2 coordinate x of P2
  /// @param _y2 coordinate y of P2
  /// @param _aa constant of the curve
  /// @param _pp the modulus
  /// @return (qx, qy) = P1+P2 in affine coordinates
  function ecAdd(
    uint256 _x1,
    uint256 _y1,
    uint256 _x2,
    uint256 _y2,
    uint256 _aa,
    uint256 _pp)
    internal pure returns(uint256, uint256)
  {
    uint x = 0;
    uint y = 0;
    uint z = 0;

    // Double if x1==x2 else add
    if (_x1==_x2) {
      // y1 = -y2 mod p
      if (addmod(_y1, _y2, _pp) == 0) {
        return(0, 0);
      } else {
        // P1 = P2
        (x, y, z) = jacDouble(
          _x1,
          _y1,
          1,
          _aa,
          _pp);
      }
    } else {
      (x, y, z) = jacAdd(
        _x1,
        _y1,
        1,
        _x2,
        _y2,
        1,
        _pp);
    }
    // Get back to affine
    return toAffine(
      x,
      y,
      z,
      _pp);
  }

  /// @dev Substract two points (x1, y1) and (x2, y2) in affine coordinates.
  /// @param _x1 coordinate x of P1
  /// @param _y1 coordinate y of P1
  /// @param _x2 coordinate x of P2
  /// @param _y2 coordinate y of P2
  /// @param _aa constant of the curve
  /// @param _pp the modulus
  /// @return (qx, qy) = P1-P2 in affine coordinates
  function ecSub(
    uint256 _x1,
    uint256 _y1,
    uint256 _x2,
    uint256 _y2,
    uint256 _aa,
    uint256 _pp)
  internal pure returns(uint256, uint256)
  {
    // invert square
    (uint256 x, uint256 y) = ecInv(_x2, _y2, _pp);
    // P1-square
    return ecAdd(
      _x1,
      _y1,
      x,
      y,
      _aa,
      _pp);
  }

  /// @dev Multiply point (x1, y1, z1) times d in affine coordinates.
  /// @param _k scalar to multiply
  /// @param _x coordinate x of P1
  /// @param _y coordinate y of P1
  /// @param _aa constant of the curve
  /// @param _pp the modulus
  /// @return (qx, qy) = d*P in affine coordinates
  function ecMul(
    uint256 _k,
    uint256 _x,
    uint256 _y,
    uint256 _aa,
    uint256 _pp)
  internal pure returns(uint256, uint256)
  {
    // Jacobian multiplication
    (uint256 x1, uint256 y1, uint256 z1) = jacMul(
      _k,
      _x,
      _y,
      1,
      _aa,
      _pp);
    // Get back to affine
    return toAffine(
      x1,
      y1,
      z1,
      _pp);
  }

  /// @dev Adds two points (x1, y1, z1) and (x2 y2, z2).
  /// @param _x1 coordinate x of P1
  /// @param _y1 coordinate y of P1
  /// @param _z1 coordinate z of P1
  /// @param _x2 coordinate x of square
  /// @param _y2 coordinate y of square
  /// @param _z2 coordinate z of square
  /// @param _pp the modulus
  /// @return (qx, qy, qz) P1+square in Jacobian
  function jacAdd(
    uint256 _x1,
    uint256 _y1,
    uint256 _z1,
    uint256 _x2,
    uint256 _y2,
    uint256 _z2,
    uint256 _pp)
  internal pure returns (uint256, uint256, uint256)
  {
    if (_x1==0 && _y1==0)
      return (_x2, _y2, _z2);
    if (_x2==0 && _y2==0)
      return (_x1, _y1, _z1);

    // We follow the equations described in https://pdfs.semanticscholar.org/5c64/29952e08025a9649c2b0ba32518e9a7fb5c2.pdf Section 5
    uint[4] memory zs; // z1^2, z1^3, z2^2, z2^3
    zs[0] = mulmod(_z1, _z1, _pp);
    zs[1] = mulmod(_z1, zs[0], _pp);
    zs[2] = mulmod(_z2, _z2, _pp);
    zs[3] = mulmod(_z2, zs[2], _pp);

    // u1, s1, u2, s2
    zs = [
      mulmod(_x1, zs[2], _pp),
      mulmod(_y1, zs[3], _pp),
      mulmod(_x2, zs[0], _pp),
      mulmod(_y2, zs[1], _pp)
    ];

    // In case of zs[0] == zs[2] && zs[1] == zs[3], double function should be used
    require(zs[0] != zs[2] || zs[1] != zs[3], "Use jacDouble function instead");

    uint[4] memory hr;
    //h
    hr[0] = addmod(zs[2], _pp - zs[0], _pp);
    //r
    hr[1] = addmod(zs[3], _pp - zs[1], _pp);
    //h^2
    hr[2] = mulmod(hr[0], hr[0], _pp);
    // h^3
    hr[3] = mulmod(hr[2], hr[0], _pp);
    // qx = -h^3  -2u1h^2+r^2
    uint256 qx = addmod(mulmod(hr[1], hr[1], _pp), _pp - hr[3], _pp);
    qx = addmod(qx, _pp - mulmod(2, mulmod(zs[0], hr[2], _pp), _pp), _pp);
    // qy = -s1*z1*h^3+r(u1*h^2 -x^3)
    uint256 qy = mulmod(hr[1], addmod(mulmod(zs[0], hr[2], _pp), _pp - qx, _pp), _pp);
    qy = addmod(qy, _pp - mulmod(zs[1], hr[3], _pp), _pp);
    // qz = h*z1*z2
    uint256 qz = mulmod(hr[0], mulmod(_z1, _z2, _pp), _pp);
    return(qx, qy, qz);
  }

  /// @dev Doubles a points (x, y, z).
  /// @param _x coordinate x of P1
  /// @param _y coordinate y of P1
  /// @param _z coordinate z of P1
  /// @param _aa the a scalar in the curve equation
  /// @param _pp the modulus
  /// @return (qx, qy, qz) 2P in Jacobian
  function jacDouble(
    uint256 _x,
    uint256 _y,
    uint256 _z,
    uint256 _aa,
    uint256 _pp)
  internal pure returns (uint256, uint256, uint256)
  {
    if (_z == 0)
      return (_x, _y, _z);

    // We follow the equations described in https://pdfs.semanticscholar.org/5c64/29952e08025a9649c2b0ba32518e9a7fb5c2.pdf Section 5
    // Note: there is a bug in the paper regarding the m parameter, M=3*(x1^2)+a*(z1^4)
    // x, y, z at this point represent the squares of _x, _y, _z
    uint256 x = mulmod(_x, _x, _pp); //x1^2
    uint256 y = mulmod(_y, _y, _pp); //y1^2
    uint256 z = mulmod(_z, _z, _pp); //z1^2

    // s
    uint s = mulmod(4, mulmod(_x, y, _pp), _pp);
    // m
    uint m = addmod(mulmod(3, x, _pp), mulmod(_aa, mulmod(z, z, _pp), _pp), _pp);

    // x, y, z at this point will be reassigned and rather represent qx, qy, qz from the paper
    // This allows to reduce the gas cost and stack footprint of the algorithm
    // qx
    x = addmod(mulmod(m, m, _pp), _pp - addmod(s, s, _pp), _pp);
    // qy = -8*y1^4 + M(S-T)
    y = addmod(mulmod(m, addmod(s, _pp - x, _pp), _pp), _pp - mulmod(8, mulmod(y, y, _pp), _pp), _pp);
    // qz = 2*y1*z1
    z = mulmod(2, mulmod(_y, _z, _pp), _pp);

    return (x, y, z);
  }

  /// @dev Multiply point (x, y, z) times d.
  /// @param _d scalar to multiply
  /// @param _x coordinate x of P1
  /// @param _y coordinate y of P1
  /// @param _z coordinate z of P1
  /// @param _aa constant of curve
  /// @param _pp the modulus
  /// @return (qx, qy, qz) d*P1 in Jacobian
  function jacMul(
    uint256 _d,
    uint256 _x,
    uint256 _y,
    uint256 _z,
    uint256 _aa,
    uint256 _pp)
  internal pure returns (uint256, uint256, uint256)
  {
    // Early return in case that `_d == 0`
    if (_d == 0) {
      return (_x, _y, _z);
    }

    uint256 remaining = _d;
    uint256 qx = 0;
    uint256 qy = 0;
    uint256 qz = 1;

    // Double and add algorithm
    while (remaining != 0) {
      if ((remaining & 1) != 0) {
        (qx, qy, qz) = jacAdd(
          qx,
          qy,
          qz,
          _x,
          _y,
          _z,
          _pp);
      }
      remaining = remaining / 2;
      (_x, _y, _z) = jacDouble(
        _x,
        _y,
        _z,
        _aa,
        _pp);
    }
    return (qx, qy, qz);
  }
}

enum GameState{ INITIAL, VOTING_STARTED, VOTING_ENDED, RESULT_PUBLISHED, RESULT_LOCKED, FUCKED_UP}

struct Point {
    uint256 x;
    uint256 y;
}

struct VoteInfo {
    Point encryptedValue;
    address owner;
    bool isBurned;
}

struct GameStateVariableSet {
    // game constants
    uint256 vGameID;
    address vHost;
    string vAssetsCID; 
    Point vPublicKey;

    uint256 vMintPrice;
    uint256 vInitPot;
    uint256 vChallengeReward;

    uint256 vMaxAllowedVotes;
    uint256 vDurationVoting;
    uint256 vTimeLimitResultPublish;
    uint256 vDurationChallenge;

    // game variables
    GameState vGameState;
    uint256 vLastStateUpdateTimeStamp;

    VoteInfo[] vVoteList;

    uint256 vPrivKey;
    uint256 vMinorityChoice;
    uint256 vVoteCntMinority;
    uint256[] vDecryptedVoteList;

    uint256 vBurnPrice;

    uint256 vStakedAmount;
    uint256 vNetPool;
}

library GameFunctions {
    event StateChanged(uint256 from, uint256 gameState);

    uint256 private constant AA = 0xffffffff00000001000000000000000000000000fffffffffffffffffffffffc;
    uint256 private constant BB = 0x5ac635d8aa3a93e7b3ebbd55769886bc651d06b0cc53b0f63bce3c3e27d2604b;
    uint256 private constant PP = 0xffffffff00000001000000000000000000000000ffffffffffffffffffffffff;
    uint256 private constant GX = 0x6b17d1f2e12c4247f8bce6e563a440f277037d812deb33a0f4a13945d898c296;
    uint256 private constant GY = 0x4fe342e2fe1a7f9b8ee7eb4a7c0f9e162bce33576b315ececbb6406837bf51f5;

    using GameFunctions for GameStateVariableSet;

    modifier onlyState(GameState curState, GameState allowedState) {
        require(curState == allowedState, "Invalid state");
        _;
    }

    modifier atleastState(GameState curState, GameState allowedState) {
        require(curState >= allowedState && curState != GameState.FUCKED_UP, "Invalid state");
        _;
    }

    /********************** 1. Generic **********************/
    function getLockedBalance(GameStateVariableSet storage self) internal view returns (uint256) {
        return self.vStakedAmount + self.vNetPool;
    }



    /********************** 2. Game State functions **********************/
    function setState(GameStateVariableSet storage self, GameState state) private {
        self.vGameState = state;
        self.vLastStateUpdateTimeStamp = block.timestamp;

        if (state == GameState.RESULT_LOCKED) {
            self.vStakedAmount = 0;
            if (self.isGameTie()) {
                uint256 burnPrice = self.vMintPrice;
                self.vNetPool = self.vVoteList.length * burnPrice;
                self.vBurnPrice = burnPrice;
            } else {
                uint256 winnerCnt = self.vVoteCntMinority;
                uint256 burnPrice = 2* self.vMintPrice + self.vInitPot / (winnerCnt == 0 ? 1: winnerCnt) ;
                self.vNetPool = winnerCnt * burnPrice;
                self.vBurnPrice = burnPrice;
            }
        } else if (state == GameState.FUCKED_UP) {
            uint256 winnerCnt = self.vVoteList.length;
            uint256 burnPrice = self.vMintPrice + (self.vInitPot + self.vStakedAmount) / (winnerCnt == 0 ? 1: winnerCnt) ;
            self.vNetPool = winnerCnt * burnPrice;
            self.vBurnPrice = burnPrice;
            self.vStakedAmount = 0;      
        }
        emit StateChanged(self.vGameID, uint256(self.vGameState));
    }

    function progressState(GameStateVariableSet storage self) internal {
        if (self.vGameState == GameState.VOTING_STARTED) {
            if (block.timestamp - self.vLastStateUpdateTimeStamp > self.vDurationVoting) {
                setState(self, GameState.VOTING_ENDED);
                return;
            }
        } else if (self.vGameState == GameState.VOTING_ENDED) {
            if (block.timestamp - self.vLastStateUpdateTimeStamp > self.vTimeLimitResultPublish) {
                setState(self, GameState.FUCKED_UP);
                return;
            }
        } else if (self.vGameState == GameState.RESULT_PUBLISHED) {
            if (block.timestamp - self.vLastStateUpdateTimeStamp > self.vDurationChallenge) {
                setState(self, GameState.RESULT_LOCKED);
                return;
            }
        }
        require(false, "cannot progress");
    }

    function canProgressState(GameStateVariableSet storage self) external view returns (bool) {
        if (self.vGameState == GameState.VOTING_STARTED) {
            if (block.timestamp - self.vLastStateUpdateTimeStamp > self.vDurationVoting) {
                return true;
            }
        } else if (self.vGameState == GameState.VOTING_ENDED) {
            if (block.timestamp - self.vLastStateUpdateTimeStamp > self.vTimeLimitResultPublish) {
                return true;
            }
        } else if (self.vGameState == GameState.RESULT_PUBLISHED) {
            if (block.timestamp - self.vLastStateUpdateTimeStamp > self.vDurationChallenge) {
                return true;
            }
        }
        return false;
    }



    /********************** 3. Host functions **********************/
    function setGameParams(GameStateVariableSet storage self, uint256 gameId, uint256 point_x, uint256 point_y,
        uint256 price, uint256 challengePenalty, string memory vAssetsCID, uint256 maxAllowedVotes, uint256 initPot,
        uint256 durationVoting, uint256 timeLimitResultPublish, uint256 durationChallenge)
        external onlyState(self.vGameState, GameState.INITIAL) 
    {
        require(EllipticCurve.isOnCurve(point_x, point_y, AA, BB, PP), "point not on curve");
        require(maxAllowedVotes >= 10000);
        self.vGameID = gameId;
        self.vHost = msg.sender;
        self.vPublicKey = Point(point_x, point_y);
        self.vMintPrice = price;
        self.vMaxAllowedVotes = maxAllowedVotes;
        self.vInitPot = initPot;
        self.vChallengeReward = challengePenalty;
        self.vAssetsCID = vAssetsCID;
        
        self.vDurationVoting = durationVoting;
        self.vTimeLimitResultPublish = timeLimitResultPublish;
        self.vDurationChallenge = durationChallenge;
    }

    function startVotingPhase(GameStateVariableSet storage self) external onlyState(self.vGameState, GameState.INITIAL) {
        require(msg.sender == self.vHost, "not host");
        uint256 challengeReward = self.vChallengeReward;
        uint256 initPot = self.vInitPot;
        require(msg.value >= challengeReward + initPot);
        setState(self, GameState.VOTING_STARTED);
        self.vNetPool = initPot;
        self.vStakedAmount = challengeReward;
    }

    function publishResult(GameStateVariableSet storage self, uint256 v0, uint256 v1, uint256 d_secret, uint256[] calldata dVotes) external onlyState(self.vGameState, GameState.VOTING_ENDED) {
        require(msg.sender == self.vHost, "not host");
        require(self.vStakedAmount + msg.value >= self.vChallengeReward, "insufficient challenge balance");
        setState(self, GameState.RESULT_PUBLISHED);

        self.vStakedAmount = self.vChallengeReward;

        self.vPrivKey = d_secret;
        if (v1 < v0 ) {
            self.vMinorityChoice = 1;
            self.vVoteCntMinority = v1;
        } else if (v1 > v0) {
            self.vMinorityChoice = 0;
            self.vVoteCntMinority = v0;
        } else {
            self.vMinorityChoice = 2;
            self.vVoteCntMinority = v0+v1;
        }
        self.vDecryptedVoteList = dVotes;
    }



    /********************** 4. Game Play functions **********************/
    function vote(GameStateVariableSet storage self, uint256 eVote_x, uint256 eVote_y) internal onlyState(self.vGameState, GameState.VOTING_STARTED) {
        require(self.vVoteList.length < self.vMaxAllowedVotes);
        uint256 mintPrice = self.vMintPrice;
        require(msg.value >= mintPrice, "insufficient amount to vote");
        require(EllipticCurve.isOnCurve(eVote_x, eVote_y, AA, BB, PP), "point not on curve");
        self.vVoteList.push(VoteInfo(Point(eVote_x, eVote_y), msg.sender, false));
        self.vNetPool = self.vNetPool + mintPrice;
    }

    function burn(GameStateVariableSet storage self, address msg_sender, uint256 tokenId, address dst) internal {
        GameState curState = self.vGameState;
        require(self.vVoteList[tokenId].owner == msg_sender, "token invalid or doesnt belong to account");
        require(self.vVoteList[tokenId].isBurned == false, "already burned");
        
        if (curState == GameState.RESULT_LOCKED) {
            require(self.isGameTie() || self.isInMinority(tokenId), "not in minority");
        } else {
            require(curState == GameState.FUCKED_UP, "Invalid state");
        }
        _burnInternalAndTransfer(self, tokenId, dst);
    }

    function _burnInternalAndTransfer(GameStateVariableSet storage self, uint256 tokenId, address dst) private {
        self.vVoteList[tokenId].isBurned = true;
        uint256 amt = self.vBurnPrice;
        self.vNetPool = self.vNetPool - amt;
        payable(dst).transfer(amt);
    }



    /********************** 5. Game View functions **********************/
    function getVotesOf(GameStateVariableSet storage self, address account, uint256 start, uint256 end) public view returns (uint256[] memory voteIdArray, bool[] memory isBurnedArray) {
        require(end > start, "invalid range");
        if (end > self.vVoteList.length) {
            end = self.vVoteList.length;
        }
        uint256 cnt =0;
        for (uint256 i=start;i<end;i++) {
            if( self.vVoteList[i].owner == account) 
                cnt++;
        }

        voteIdArray = new uint256[](cnt);
        isBurnedArray = new bool[](cnt);
        uint256 ptr =0;
        for (uint256 i=start;i<end;i++) {
            if(self.vVoteList[i].owner == account) {
                isBurnedArray[ptr] = self.vVoteList[i].isBurned;
                voteIdArray[ptr++] = i;
            }
        }
    }

    function getDecryptedVote(GameStateVariableSet storage self, uint256 voteId) internal view atleastState(self.vGameState, GameState.RESULT_PUBLISHED) returns (uint256) {
        return (self.vDecryptedVoteList[voteId/256] & ( 1<<(255- voteId%256) ) == 0) ? 0: 1 ;
    }

    function isInMinority(GameStateVariableSet storage self, uint256 voteId) internal view atleastState(self.vGameState, GameState.RESULT_PUBLISHED) returns (bool) {
        return self.getDecryptedVote(voteId) == self.vMinorityChoice;
    }
    
    function getWinningVotesOf(GameStateVariableSet storage self,address account, uint256 start, uint256 end) public view returns (uint256[] memory winningVoteIdList, bool[] memory winningIsBurnedList) {
        require(self.vGameState >= GameState.RESULT_PUBLISHED);
        (uint256[] memory voteIdList, bool[] memory isBurnedList) = getVotesOf(self, account, start, end);
        if (self.vGameState == GameState.FUCKED_UP || self.isGameTie()) {
            return (voteIdList, isBurnedList);
        }
        uint256 cnt =0;
        for (uint256 i=0;i<voteIdList.length;i++) {
            uint256 tokenId = voteIdList[i];
            if(isInMinority(self, tokenId)) {
                isBurnedList[cnt] = isBurnedList[i];
                voteIdList[cnt++] = tokenId;
            }
        }

        winningVoteIdList = new uint256[](cnt);
        winningIsBurnedList = new bool[](cnt);

        for (uint256 i=0;i<cnt;i++) {
            winningVoteIdList[i] = voteIdList[i];
            winningIsBurnedList[i] = isBurnedList[i];
        }
    }

    function isGameTie(GameStateVariableSet storage self) internal view atleastState(self.vGameState, GameState.RESULT_PUBLISHED) returns (bool) {
        return self.vMinorityChoice == 2;
    }



    /********************** 6. Challenge functions (View) **********************/
    function checkPrivateKey(GameStateVariableSet storage self) public view atleastState(self.vGameState, GameState.RESULT_PUBLISHED) returns (bool) {
        (uint256 x, uint256 y) = EllipticCurve.ecMul(self.vPrivKey, GX, GY, AA, PP);
        return (x==self.vPublicKey.x && y == self.vPublicKey.y);
    }

    function checkVoteCounting(GameStateVariableSet storage self) public view atleastState(self.vGameState, GameState.RESULT_PUBLISHED) returns (bool) {
        uint256 cnt1 = 0;
        for (uint256 i=0; i<self.vDecryptedVoteList.length; i++) {
            uint256 setBitsCnt = 0;
            uint256 word = self.vDecryptedVoteList[i];

            while(word > 0) {
                word = word & (word-1);
                setBitsCnt++;
            }
            cnt1 += setBitsCnt;
        }

        uint256 cnt0 = self.vVoteList.length - cnt1;

        if (cnt1 < cnt0) {
            return self.vMinorityChoice == 1 && self.vVoteCntMinority == cnt1;
        } else if (cnt1 > cnt0) {
            return self.vMinorityChoice == 0 && self.vVoteCntMinority == cnt0;
        } else {
            return self.vMinorityChoice == 2;
        }
    }

    function checkDecryption(GameStateVariableSet storage self, uint256 voteId) public view atleastState(self.vGameState, GameState.RESULT_PUBLISHED) returns (bool) {
        uint256 expectedDecryptedVote = decryptVote(self, self.vVoteList[voteId].encryptedValue.x, self.vVoteList[voteId].encryptedValue.y);
        uint256 publishedDecryptedVote = self.getDecryptedVote(voteId);
        return expectedDecryptedVote == publishedDecryptedVote;
    }



    /********************** 7. Challenge functions (Write) **********************/
    function challengePrivateKeyLeak(GameStateVariableSet storage self, uint256 privKey) external onlyState(self.vGameState, GameState.VOTING_STARTED) {
        (uint256 x, uint256 y) = EllipticCurve.ecMul(privKey, GX, GY, AA, PP);
        require(x==self.vPublicKey.x && y == self.vPublicKey.y, "challenge failed");
        setState(self, GameState.FUCKED_UP);
        payThatManHisMoney(self.vChallengeReward);
    }

    function challengePrivateKey(GameStateVariableSet storage self) external onlyState(self.vGameState, GameState.RESULT_PUBLISHED) {
        require(!checkPrivateKey(self), "challenge failed");
        self.vStakedAmount = 0;
        setState(self, GameState.VOTING_ENDED);
        payThatManHisMoney(self.vChallengeReward);
    }

    function challengeVoteCounting(GameStateVariableSet storage self) external onlyState(self.vGameState, GameState.RESULT_PUBLISHED) {
        require(!checkVoteCounting(self), "challenge failed");
        self.vStakedAmount = 0;
        setState(self, GameState.VOTING_ENDED);
        payThatManHisMoney(self.vChallengeReward);
    }

    function challengeDecryption(GameStateVariableSet storage self, uint256 voteId) external onlyState(self.vGameState, GameState.RESULT_PUBLISHED) {
        require(!checkDecryption(self, voteId), "challenge failed");
        self.vStakedAmount = 0;
        setState(self, GameState.VOTING_ENDED);
        payThatManHisMoney(self.vChallengeReward);
    }
    


    /********************** 8. Utility functions **********************/
    function decryptVote(GameStateVariableSet storage self, uint256 x, uint256 y) public view atleastState(self.vGameState, GameState.RESULT_PUBLISHED) returns (uint256) {
        (uint256 x1,) = EllipticCurve.ecMul(self.vPrivKey, x, y, AA, PP);
        return x1%2;
    }

    function payThatManHisMoney(uint256 amt) private {
        payable(msg.sender).transfer(amt);
    }

}

contract Ownable {
    address private _owner;
    mapping(address => bool) private _isHost;

    constructor() {
        _owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, "Not owner");
        _;
    }

    modifier onlyHost() {
        require(_isHost[msg.sender], "Not host");
        _;
    }

    function getOwner() external view returns (address) {
        return _owner;
    }

    function changeHostPrivileges(address addr, bool makeHost) external onlyOwner {
        _isHost[addr] = makeHost;
    }
}

contract MinorityGameManager is Ownable, AutomationCompatibleInterface {
    GameStateVariableSet[] private _gameStateList;
    uint256 private _lockedBalance;

    uint256 _durationVoting = 24*60*60; //24 hours
    uint256 _timeLimitResultPublish = 12*60*60; //12 hours
    uint256 _durationChallenge = 12*60*60; //12 hours

    using GameFunctions for GameStateVariableSet;

    modifier updateLockedBalance(uint256 gameId) {
        uint256 gameLockedBalBefore = _gameStateList[gameId].getLockedBalance();
        _;
        uint256 gameLockedBalAfter = _gameStateList[gameId].getLockedBalance();
        if (gameLockedBalBefore != gameLockedBalAfter) {
            _lockedBalance = (_lockedBalance + gameLockedBalAfter) - gameLockedBalBefore;
        }
    }

    /************* 1. top level functions*************/
    function getGamesCount() external view returns (uint256) {
        return _gameStateList.length;
    }

    function createGame(uint256 point_x, uint256 point_y, uint256 price, uint256 challengePenalty, string calldata vAssetsCID, uint256 maxAllowedVotes, uint256 initPot) external onlyHost {
        _gameStateList.push();
        uint256 gameId = _gameStateList.length - 1;
        _gameStateList[gameId].setGameParams(gameId, point_x, point_y, price, challengePenalty, vAssetsCID, maxAllowedVotes, initPot,
            _durationVoting, _timeLimitResultPublish, _durationChallenge);
    }

    function startVotingPhase(uint256 gameId) external payable updateLockedBalance(gameId) {
        _gameStateList[gameId].startVotingPhase();
    }

    function publishResult(uint256 gameId, uint256 v0, uint256 v1, uint256 d_secret, uint256[] calldata dVotes) external payable updateLockedBalance(gameId) {
        _gameStateList[gameId].publishResult(v0, v1, d_secret, dVotes);
    }

    function setGameTimeParams(uint256 durationVoting, uint256 timeLimitResultPublish, uint256 durationChallenge) external onlyOwner {
        _durationVoting = durationVoting;
        _timeLimitResultPublish = timeLimitResultPublish;
        _durationChallenge = durationChallenge;
    }

    /************* 2. game write functions *************/
    function vote(uint256 gameId, uint256 eVote_x, uint256 eVote_y) payable external {
        _gameStateList[gameId].vote(eVote_x, eVote_y);
        _lockedBalance = _lockedBalance + _gameStateList[gameId].vMintPrice;
    }

    function voteMultiple(uint256 gameId, Point[] memory encryptedVoteList) payable external updateLockedBalance(gameId) {
        require(encryptedVoteList.length > 0, "no. of votes out of bound");
        GameStateVariableSet storage gameVariableSet = _gameStateList[gameId];
        require(msg.value >= gameVariableSet.vMintPrice*encryptedVoteList.length, "insufficient amount to vote");
        for (uint256 i =0; i< encryptedVoteList.length; i++) {
            gameVariableSet.vote(encryptedVoteList[i].x, encryptedVoteList[i].y);
        }
    }

    function burn(uint256 gameId, uint256 tokenId) external {
        _gameStateList[gameId].burn(msg.sender, tokenId, msg.sender);
        _lockedBalance = _lockedBalance - _gameStateList[gameId].vBurnPrice;
    }

    function burnMultiple(uint256 gameId, uint256[] calldata tokenIdList, address dst) external updateLockedBalance(gameId) {
        GameStateVariableSet storage gameVariableSet = _gameStateList[gameId];
        require(tokenIdList.length > 0, "no. of votes out of bound");
        for (uint256 i =0; i< tokenIdList.length; i++) {
            gameVariableSet.burn(msg.sender, tokenIdList[i], dst);
        }
    }



    /************* 3. keeper functions/ game state updation *************/

    function checkUpkeep(bytes calldata /*checkData*/) external view override returns (bool upkeepNeeded, bytes memory performData) {
        uint256 gameId;
        uint256 end = _gameStateList.length;
        uint256 start = end <= 100? 0: end-100;
        (upkeepNeeded, gameId) = checkCanProgressStateForAnyGame(start, end);
        performData = abi.encodePacked(gameId);
    }

    function performUpkeep(bytes calldata performData) external override {
        uint256 gameId = abi.decode(performData, (uint256));
        require(gameId < _gameStateList.length);
        progressState(gameId);
    }

    function checkCanProgressStateForAnyGame(uint256 start, uint256 end) public view returns (bool canProgress, uint256 gameId) {
        canProgress = false;
        require(end > start, "invalid range");
        require(end <= _gameStateList.length);
    
        for (uint256 i=start; i<end; i++) {
            if (_gameStateList[i].canProgressState()) {
                canProgress = true;
                gameId = i;
                break;
            }
        }
    }

    function progressState(uint256 gameId) public updateLockedBalance(gameId) {
        _gameStateList[gameId].progressState();
    }



    /************* 3. Game View functions *************/
    
    // 3.a CAN BE CALLED IN ANY GAME STATE
    function getGameStateInfo(uint256 gameId) external view returns (uint256 gameState, uint256 lastStateUpdateTimeStamp) {
        return (uint256(_gameStateList[gameId].vGameState), _gameStateList[gameId].vLastStateUpdateTimeStamp);
    }

    function getGameParams(uint256 gameId) external view returns (address host, string memory assetsCID, Point memory publicKey,
        uint256 mintPrice, uint256 initialPot, uint256 challengeReward, uint256 maxAllowedVotes,
        uint256 durationVoting, uint256 timeLimitResultPublish, uint256 durationChallenge) {
        GameStateVariableSet storage gameState = _gameStateList[gameId];
        return (gameState.vHost, gameState.vAssetsCID, gameState.vPublicKey, 
            gameState.vMintPrice, gameState.vInitPot, gameState.vChallengeReward, gameState.vMaxAllowedVotes,
            gameState.vDurationVoting, gameState.vTimeLimitResultPublish, gameState.vDurationChallenge);
    }

    function getNumberOfMints(uint256 gameId) external view returns (uint256) {
        return _gameStateList[gameId].vVoteList.length;
    }

    function getOwnerAddressForVoteId(uint256 gameId, uint256 voteId) external view returns (address) {
        return _gameStateList[gameId].vVoteList[voteId].owner;
    }

    function getEncryptedVoteForVoteId(uint256 gameId, uint256 voteId) external view returns (Point memory) {
        return _gameStateList[gameId].vVoteList[voteId].encryptedValue;
    }

    function getEncryptedVoteList(uint256 gameId, uint256 start, uint256 end) external view returns (Point[] memory) {
        VoteInfo[] storage encryptedVoteList = _gameStateList[gameId].vVoteList;
        require(start < end, "invalid range");
        if (end > encryptedVoteList.length) {
            end = encryptedVoteList.length;
        }    
        Point[] memory subArray = new Point[](end-start);
        for (uint256 i = start; i < end; i++) {
            subArray[i - start] = encryptedVoteList[i].encryptedValue;
        }
        return subArray;
    }

    function getVotesOf(uint256 gameId, address account, uint256 start, uint256 end) external view returns (uint256[] memory voteIdArray)
    {
        (voteIdArray, ) = _gameStateList[gameId].getVotesOf(account, start, end);
    }

    // 3.b AFTER GAME RESULTS PUBLISHED
    function getGamePrivateKey(uint256 gameId) external view returns (uint256) {
        GameState curState = _gameStateList[gameId].vGameState;
        require(curState >= GameState.RESULT_PUBLISHED && curState != GameState.FUCKED_UP, "Invalid state");
        return _gameStateList[gameId].vPrivKey;
    }

    function getWinningChoiceInfo(uint256 gameId) external view returns (uint256 minorityChoice, uint256 voteCountMinority) {
        GameState curState = _gameStateList[gameId].vGameState;
        require(curState >= GameState.RESULT_PUBLISHED && curState != GameState.FUCKED_UP, "Invalid state");
        return (_gameStateList[gameId].vMinorityChoice, _gameStateList[gameId].vVoteCntMinority);
    }

    function getGameBurnPrice(uint256 gameId) external view returns (uint256) {
        GameState curState = _gameStateList[gameId].vGameState;
        require(curState >= GameState.RESULT_LOCKED, "Invalid state");
        return _gameStateList[gameId].vBurnPrice;
    }

    function getDecryptedVote(uint256 gameId, uint256 voteId) external view returns (uint256) {
        return _gameStateList[gameId].getDecryptedVote(voteId);
    }

    function isInMinority(uint256 gameId, uint256 voteId) external view returns (bool) {
        return _gameStateList[gameId].isInMinority(voteId);
    }

    function getWinningVotesOf(uint256 gameId, address account, uint256 start, uint256 end) external view returns (uint256[] memory, bool[] memory) {
        return _gameStateList[gameId].getWinningVotesOf(account, start, end);
    }



    /************* 4. CHALLENGE functions *************/

    function challengeLeakPrivateKey(uint256 gameId, uint256 privKey) external updateLockedBalance(gameId) {
        _gameStateList[gameId].challengePrivateKeyLeak(privKey);
    }

    function challengeResultPrivateKey(uint256 gameId) external updateLockedBalance(gameId) {
        _gameStateList[gameId].challengePrivateKey();
    }

    function challengeResultVoteCounting(uint256 gameId) external updateLockedBalance(gameId) {
        _gameStateList[gameId].challengeVoteCounting();
    }

    function challengeResultDecryption(uint256 gameId, uint256 voteId) external updateLockedBalance(gameId) {
        _gameStateList[gameId].challengeDecryption(voteId);
    }



    /************* 5. UTILITY functions *************/

    function decryptVote(uint256 gameId, uint256 x, uint256 y) external view returns (uint256) {
        return _gameStateList[gameId].decryptVote(x,y);
    }



    /************* 6. MISC *************/
    fallback() external payable {
    }

    receive() external payable {
    }

    function withdraw(address dst, uint256 amount) external onlyOwner {
        require(amount + _lockedBalance <= address(this).balance, "amount too high");
        payable(dst).transfer(amount);
    }

    function getLockedBalance() external view returns (uint256) {
        return _lockedBalance;
    }
}