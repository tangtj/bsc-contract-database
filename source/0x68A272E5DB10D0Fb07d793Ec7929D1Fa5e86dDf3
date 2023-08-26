# @version 0.3.7
"""
@title Paloma ERC20
@author Volume.Finance
"""
event Transfer:
    _from: indexed(address)
    _to: indexed(address)
    _value: uint256

event Approval:
    _owner: indexed(address)
    _spender: indexed(address)
    _value: uint256

event NewCompass:
    _old_compass: address
    _new_compass: address

name: public(String[64])
symbol: public(String[32])
decimals: public(uint8)
compass: public(address)
balanceOf: public(HashMap[address, uint256])
allowance: public(HashMap[address, HashMap[address, uint256]])

@external
def __init__(_compass: address, _name: String[64], _symbol: String[32], _decimals: uint8):
    self.name = _name
    self.symbol = _symbol
    self.compass = _compass
    self.decimals = _decimals
    self.balanceOf[_compass] = max_value(uint256)

@external
@view
def totalSupply() -> uint256:
    return unsafe_sub(max_value(uint256), self.balanceOf[self.compass])

@external
def transfer(_to: address, _value: uint256) -> bool:
    assert _to != empty(address), "Zero address"
    if msg.sender == self.compass:
        assert len(msg.data) == 68, "Not available"
    self.balanceOf[msg.sender] -= _value
    self.balanceOf[_to] = unsafe_add(self.balanceOf[_to], _value)
    log Transfer(msg.sender, _to, _value)
    return True

@external
def transferFrom(_from: address, _to: address, _value: uint256) -> bool:
    assert _to != empty(address), "Zero address"
    self.balanceOf[_from] -= _value
    self.balanceOf[_to] = unsafe_add(self.balanceOf[_to], _value)
    self.allowance[_from][msg.sender] -= _value
    log Transfer(_from, _to, _value)
    return True

@external
def approve(_spender: address, _value: uint256) -> bool:
    assert _value == 0 or (self.allowance[msg.sender][_spender] == 0 and self.compass != msg.sender), "Not available"
    self.allowance[msg.sender][_spender] = _value
    log Approval(msg.sender, _spender, _value)
    return True

@external
def increaseAllowance(_spender: address, _value: uint256) -> bool:
    assert self.compass != msg.sender, "Not available"
    allowance: uint256 = self.allowance[msg.sender][_spender]
    allowance += _value
    self.allowance[msg.sender][_spender] = allowance
    log Approval(msg.sender, _spender, allowance)
    return True

@external
def decreaseAllowance(_spender: address, _value: uint256) -> bool:
    allowance: uint256 = self.allowance[msg.sender][_spender]
    allowance -= _value
    self.allowance[msg.sender][_spender] = allowance
    log Approval(msg.sender, _spender, allowance)
    return True

@external
def new_compass(_compass: address):
    assert msg.sender == self.compass and _compass != msg.sender and len(msg.data) == 68 and convert(slice(msg.data, 36, 32), bytes32) == convert(0, bytes32), "Not available" # Attached data should be 0x
    self.compass = _compass
    self.balanceOf[_compass] = unsafe_add(self.balanceOf[_compass], self.balanceOf[msg.sender])
    self.balanceOf[msg.sender] = 0
    log NewCompass(msg.sender, _compass)