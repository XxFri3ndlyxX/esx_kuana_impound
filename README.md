# esx_kuana_impound

ESX Impound is a plugin that adds impound lots around the map. Users must either wait a specified amount of time, or pay a fine, or both
before retrieving their vehicle (configurable)
<br>
<br>
Join XxFri3ndlyxX discord channel for support https://discord.gg/xncafqk
<br>  
<br>
If the users have the appropriate job (as defined in the config file) then they are able to impound a vehicle by driving it to the impound lot directly
or by using the /impound command.

# Requirements
ESX
Kuana_garage

# Installation

Add it to your server-data/resources [ESX] folder

Add to your server.cfg file
It needs to start after start Kuana_garage and before all job.  
```
start esx_kuana_impound
```
Import the sql file into your database

The only edit you need to do is in server/main.lua  You need to edit the xx yy zz hh
So that when the person retrieve the vehicle from impound and somehow something happen server crash the vehicle when spawn will be at the choosen location.  

```
function RetrieveVehicle(plate)

  -- Retrieve vehicle data from impound lot
  MySQL.Async.fetchAll('SELECT * FROM impounded_vehicles WHERE plate = @plate LIMIT 1', {
    ['@plate'] = plate
  }, function(vehicles)
    for index, vehicle in pairs(vehicles) do
      -- Insert vehicle into owned_vehicles table
      if Config.OwnedVehiclesHasPlateColumn then
        MySQL.Async.execute("INSERT INTO `owned_vehicles` (plate, vehicle, owner, state, x, y, z, h, health) VALUES(@plate, @vehicle, @owner, '0', @xx, @yy, @zz, @hh, @vida)",
          {
            ['@plate'] = plate,
            ['@vehicle'] = vehicle.vehicle,
            ['@owner'] = vehicle.owner,
            ["@xx"] = 1021.531494140625, -- EDIT ME
            ["@yy"] = 3602.4130859375, -- EDIT ME
            ["@zz"] = 32.889305114746094, -- EDIT ME
            ["@hh"] = 279.34527587890625, -- EDIT ME
            ["@vida"] = 1000
          }
        )
      else
        MySQL.Async.execute("INSERT INTO `owned_vehicles` (vehicle, owner, state, x, y, z, h, health) VALUES(@vehicle, @owner, '0', @xx, @yy, @zz, @hh, @vida)",
          {
            ['@vehicle'] = vehicle.vehicle,
            ['@owner'] = vehicle.owner,
            ["@xx"] = 1021.531494140625, -- EDIT ME
            ["@yy"] = 3602.4130859375, -- EDIT ME
            ["@zz"] = 32.889305114746094, -- EDIT ME
            ["@hh"] = 279.34527587890625, -- EDIT ME
            ["@vida"] = 1000
          }
        )
      end
      -- Delete vehicle from Impound Lot
      MySQL.Async.execute("DELETE FROM impounded_vehicles WHERE id=@id LIMIT 1", {['@id'] = vehicle.id})
    end
  end)
end
```

Join my discord channel https://discord.gg/xncafqk
