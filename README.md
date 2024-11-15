Simple system to use only one RemoteEvent in your game, sending data based on keys.

---
## Installation

Put `PortableEvent.rbxm` in `ReplicatedStorage` and require the module:

```lua
-- Server
local PortableEvent = require(game.ReplicatedStorage.PortableEvent)

-- Client
local PortableEvent = require(game.ReplicatedStorage:WaitForChild('PortableEvent'))
```

---
# Client:
## Client Functions

- **sendToServer** `(key: string, ...: any): void`
*Send data to the server.*

## Client Methods

- **clientResponse** `(key: string, Function: (any) -> (any)): void`
*Receives data sent by the server.*

---
# Server:

## Server Functions

- **sendToAllClients** `(key: string, ...: any): void`
*Send data to all players.*

- **sendToClient** `(player: Player, key: string, ...: any): void`
*Send data to a specific player.*

## Server Methods

- **serverResponse** `(key: string, Function: (player: Player, ...any) -> (any)): void`
*Receives data sent by the client.*

---
# Examples 1 (Printing Message):

**Client:**
```lua
local PortableEvent = require(game.ReplicatedStorage:WaitForChild('PortableEvent'))
local Key = 'JoinMessage'

PortableEvent:clientResponse(Key, function(Message: string)
	-- Printing message
	print(Message) -- Output: Welcome to the Game!
end)
```

**Server:**
```lua
local Players = game:GetService('Players')

local PortableEvent = require(game.ReplicatedStorage.PortableEvent)

local function PlayerAdded(player)
	local Key = 'JoinMessage'
	local Message = 'Welcome to the Game!'
	
	-- Sending message to player with key 'JoinMessage'
	PortableEvent.sendToClient(player, Key, Message)
end

Players.PlayerAdded:Connect(PlayerAdded)
```

# Example 2 (Using Power):

**Client:**
```lua
local PortableEvent = require(game.ReplicatedStorage.PortableEvent)

-- Powers list
local Powers = {
	'Fire',
	'Water',
	'Wind'
}

local function UsePower(name: string, state: Enum.UserInputState, Input: InputObject)
	if state == Enum.UserInputState.End or state == Enum.UserInputState.Cancel then return end
	
	-- Random power
	local power = Powers[math.random(1, #Powers)]
	
	-- Sending power to the server
	PortableEvent.sendToServer('UsePower', power)
end

game:GetService('ContextActionService'):BindAction('LaunchPower', UsePower, false, Enum.KeyCode.E)
```

**Server:**
```lua
local PortableEvent = require(game.ReplicatedStorage.PortableEvent)

-- Receiving power data
PortableEvent:serverResponse('UsePower', function(player: Player, power: string)
	print(`{player.DisplayName:upper()} used the power {power:upper()}`)
end)
```
