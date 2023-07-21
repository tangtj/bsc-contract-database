pragma solidity 0.5.16;

interface IBEP20 {
    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);
}

library DateTime {
    uint constant SECONDS_PER_DAY = 24 * 60 * 60;
    uint constant SECONDS_PER_HOUR = 60 * 60;
    uint constant SECONDS_PER_MINUTE = 60;
    int constant OFFSET19700101 = 2440588;

    function _daysFromDate(uint year, uint month, uint day) internal pure returns (uint _days) {
        require(year >= 1970);
        int _year = int(year);
        int _month = int(month);
        int _day = int(day);

        int __days = _day
            - 32075
            + 1461 * (_year + 4800 + (_month - 14) / 12) / 4
            + 367 * (_month - 2 - (_month - 14) / 12 * 12) / 12
            - 3 * ((_year + 4900 + (_month - 14) / 12) / 100) / 4
            - OFFSET19700101;

        _days = uint(__days);
    }

    function _daysToDate(uint _days) internal pure returns (uint year, uint month, uint day) {
        int __days = int(_days);

        int L = __days + 68569 + OFFSET19700101;
        int N = 4 * L / 146097;
        L = L - (146097 * N + 3) / 4;
        int _year = 4000 * (L + 1) / 1461001;
        L = L - 1461 * _year / 4 + 31;
        int _month = 80 * L / 2447;
        int _day = L - 2447 * _month / 80;
        L = _month / 11;
        _month = _month + 2 - 12 * L;
        _year = 100 * (N - 49) + _year + L;

        year = uint(_year);
        month = uint(_month);
        day = uint(_day);
    }

    function timestampFromDate(uint year, uint month, uint day) internal pure returns (uint timestamp) {
        timestamp = _daysFromDate(year, month, day) * SECONDS_PER_DAY;
    }

    function timestampFromDateTime(uint year, uint month, uint day, uint hour, uint minute, uint second) internal pure returns (uint timestamp) {
        timestamp = _daysFromDate(year, month, day) * SECONDS_PER_DAY + hour * SECONDS_PER_HOUR + minute * SECONDS_PER_MINUTE + second;
    }

    function timestampToDate(uint timestamp) internal pure returns (uint year, uint month, uint day) {
        (year, month, day) = _daysToDate(timestamp / SECONDS_PER_DAY);
    }

    function timestampToDateTime(uint timestamp) internal pure returns (uint year, uint month, uint day, uint hour, uint minute, uint second) {
        (year, month, day) = _daysToDate(timestamp / SECONDS_PER_DAY);
        uint secs = timestamp % SECONDS_PER_DAY;
        hour = secs / SECONDS_PER_HOUR;
        secs = secs % SECONDS_PER_HOUR;
        minute = secs / SECONDS_PER_MINUTE;
        second = secs % SECONDS_PER_MINUTE;
    }

    function diffDays(uint fromTimestamp, uint toTimestamp) internal pure returns (uint _days) {
        require(fromTimestamp <= toTimestamp);
        _days = (toTimestamp - fromTimestamp) / SECONDS_PER_DAY;
    }

    function diffMinutes(uint fromTimestamp, uint toTimestamp) internal pure returns (uint _minutes) {
        require(fromTimestamp <= toTimestamp);
        _minutes = (toTimestamp - fromTimestamp) / SECONDS_PER_MINUTE;
    }

}

//https://tool.ip138.com/timestamp/
contract C {
    using DateTime for uint;
    address[4] private _wallets = [
    0x15c380b48D792Ead51568826c702c8a126d87875,
    0x97ea19128D9a1875a37beaD4d807E17Bf41fdff9,
    0x679b8C5d882607eE6f9e2ba9093c839a11B298De,
    0x90af05a949Abd7413b05Fd512A173731C35E9cFe
    ];
    uint256[4] private _values_a = [
    uint256(192157000000000000000000),
    uint256(1100000000000000000000),
    uint256(600000000000000000000),
    uint256(400000000000000000000)
    ];
    uint256[4] private _values_b = [
    uint256(20580000000000000000),
    uint256(11000000000000000),
    uint256(6000000000000000),
    uint256(4000000000000000)
    ];
    uint private _lastTimestamp;
    uint private _totalDay = 1000;
    uint private _lastDay = 0;
    IBEP20 private _token_a = IBEP20(0x79EB84edA07bB77923a43b5632418056B2bA9424);
    IBEP20 private _token_b = IBEP20(0x88F26A6d337b4A70e6280Ecfaa002e079edd00b4);

    constructor(uint startTime) public {
        _lastTimestamp = startTime;
    }

    function blockTimestamp() public view returns (uint256) {
        return block.timestamp;
    }

    function lastTimestamp() public view returns (uint256) {
        return _lastTimestamp;
    }

    function totalDay() public view returns (uint256) {
        return _totalDay;
    }

    function lastDay() public view returns (uint256) {
        return _lastDay;
    }

    function diffDays() public view returns (uint) {
        if (isOver()) return 0;
        return DateTime.diffDays(_lastTimestamp, block.timestamp);
    }

    function isOver() public view returns (bool) {
        return _lastDay >= _totalDay;
    }

    function release() public returns (bool) {
        uint diff = diffDays();
        require(diff > 0, "time did not arrive");
        if (diff > 0) {
            uint day = _lastDay;
            for (uint i = 0; i < diff; i++) {
                if (day >= _totalDay) break;
                day++;
                for (uint j = 0; j < _wallets.length; j++) {
                    _token_a.transfer(_wallets[j], _values_a[j]);
                    _token_b.transfer(_wallets[j], _values_b[j]);
                }
            }
            _lastDay = day;
            (uint _year, uint _month, uint _day) = DateTime.timestampToDate(block.timestamp);
            _lastTimestamp = DateTime.timestampFromDate(_year, _month, _day);
            return true;
        }
        return false;
    }

}