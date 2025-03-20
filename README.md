This mod adds telekinetic abilities to your character in Lethal Company, allowing you to grab and throw objects or enemies using psychic powers!

## Features
- Grab objects & enemies with E key
- Throw objects with F key
- Carry up to 2 objects at once (configurable)
- Customizable grab range and throw force
- Visual effects (particles and glow)
- Sound effects
- Network synchronized for multiplayer

## Configuration
The mod includes several configurable settings in the BepInEx config file:

### General Settings
- `GrabRange`: Distance within which you can grab objects (1-10 units)
- `ThrowForce`: Strength of the throw (1-20 force units)
- `MaxGrabbedObjects`: Maximum number of objects you can carry (1-5)

### Visual Effects
- `EnableVisualEffects`: Toggle particle effects and glow
- `UpdateRate`: How frequently grabbed objects update their position

### Sound Effects
- `EnableSoundEffects`: Toggle sound effects for grabbing and throwing

## Requirements
- BepInEx
- HarmonyLib
- Lethal Company

## Installation
1. Install BepInEx to your Lethal Company installation
2. Place the mod DLL in the BepInEx/plugins folder
3. Configure settings in BepInEx/config/TelekinesisMod.cfg

## Compilation
1. Open the solution in Visual Studio 2022 or later
2. Build the project in Release configuration
3. The DLL will be output to the `bin\Release\netstandard2.0` folder

## Usage
- Press E to grab objects within range
- Press F to throw grabbed objects
- Objects will glow cyan when grabbed
- Particle effects will appear when grabbing and throwing

## Credits
- Airzel and zeeblo - Mod author
