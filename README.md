# Twilight Changelog
### Twilight Update: TwLoader 1.9.9 - November 26, 2024
#### Changes and Fixes:
1. Fixed performance issues when using the menu.

---
### Twilight Update: TwLoader 1.9.8 | TwKernelBridge - November 24, 2024
#### Changes and Fixes:
1. Fixed duplicated hero rendering image.
2. Fixed UI improvements. The UI is now draggable.
3. Removed Draw HP Pack, Box, and Distance.
4. Sojourn bot now has speed/acceleration sliders.
5. Added Hazard (New Tank) settings.


### New:
1. Added Tracer Auto Recall.
2. Added Junker Queen Auto Commanding Shout.
3. Added Genji Auto Deflect.
4. Added Zarya Auto Bubble.
5. Added Moira Auto Fade.

### TwKernelBridge: 
Twilight Temp Spoofer is updating, no ETA.

---
### TwKernelBridge Update & Announcement - November 19, 2024
We received two ban reports yesterday. As a precautionary measure, we temporarily suspended Twilight to ensure there were no detections on our side.

After thoroughly testing both users' configurations and our product in rage mode for three hours, we confirmed everything is functioning as intended. The bans were caused by user abuse (rage behavior) and not due to any issues with our development. However, we’ve always been transparent with our community - unlike others - and want to share a few key methods we use to keep you protected from detection:

#### Callstack spoofing

We execute each function indirectly instead of calling them directly. At runtime, we generate shellcode and allocate executable memory to copy and execute the function. Each function is executed in a separate memory region, ensuring no suspicious traces. Here’s the function signature:
```C++
TwShellCodeGenerator(Func f, Args&... args)
```
#### Custom Mouse Events

We don’t use Windows API mouse events. Instead, our private implementation ensures that no Warden kernel modules are triggered. For instance:
```C++
VOID TwSendMouseEventEx(DWORD dwFlags, DWORD dx, DWORD dy, DWORD dwData, ULONG_PTR dwExtraInfo) 
{
    INPUT Input[3] = { 0 };
    Input[0].type = INPUT_MOUSE;
    Input[0].mi.dx = dx;
    Input[0].mi.dy = dy;
    Input[0].mi.mouseData = dwData;
    Input[0].mi.dwFlags = dwFlags;
    Input[0].mi.time = 0;
    Input[0].mi.dwExtraInfo = dwExtraInfo;

    _NtUserSendInput((UINT)1, (LPINPUT)&Input, (INT)sizeof(INPUT));
}
```
With the internal implementation:
```asm
_NtUserSendInput PROC
    mov r10, rcx
    mov eax, 3735928559
    syscall
    ret
_NtUserSendInput ENDP
END
```
#### Custom Hashing Algorithm

We use a proprietary algorithm to hash each line of code, ensuring complexity and obfuscation:
```C++
TwGenHash(std::string block)
```

#### Custom XOR and LLVM Integration

To further enhance security, we employ a custom XOR mechanism and integrate LLVM-based obfuscation.

5. **Legitimate Signed Drivers**

We load legitimate, signed kernel drivers. Our `TwKernelBridge` load these drivers as normal drivers using the `NtLoader` function, ensuring seamless integration.

#### Advanced Low-Level Spoofing

Our spoofing techniques go beyond the limits of Warden and Eidolon. An example:


```asm
_TwSpoof proc
    pop r11
    add rsp, 8
    mov rax, [rsp + 24]

    mov r10, [rax]
    mov [rsp], r10

    mov r10, [rax + 8]
    mov [rax + 8], r11

    mov [rax + 16], rbx
    lea rbx, fixup
    mov [rax], rbx
    mov rbx, rax

    jmp r10

fixup:
    sub rsp, 16
    mov rcx, rbx
    mov rbx, [rcx + 16]
    jmp QWORD PTR [rcx + 8]
_TwSpoof_stub endp
end
```
Please note that these are just examples of the methods we employ, not a comprehensive list. Since day one, **we’ve prioritized user safety and experience above all else**. With this in mind, we’ve released a new update to our `TwKernelBridge`, adding an additional security layer to reinforce user protection.

In addition, we’ve given each user 6 extra hours of time to make up for the temporary suspension.

**Twilight will always have your back.**

_Best regards,_


_Twilight Solutions_

---
### Twilight 1.9.7 & Our Newest Mapper TwKernelBridge - November 18, 2024
#### Changes and Fixes:
1. Fixed prediction issues.
2. Removed the team outline feature.
3. Removed skeleton-related features, including skeleton thickness and dots.
4. Removed ESP colors (Skeleton Vis/Invis and Teammates Color).
5. Removed the rainbow outline feature.
6. Removed hitbox resizing from the primary and secondary aimbot.
7. Removed ESP colors.
8. Removed Hanzo auto-prediction.
9. Added a new no-recoil system.
10. Reintroduced the legacy no-recoil (old system).
11. Added HP Pack drawing.
12. Added HP Pack Distance.
13. Added HP Pack Box.
14. Fixed DSE with `SpTwilight.sys`.
15. `TwsCore.sys` driver has been replaced with our newest kernel driver `TwCmdCtrl.sys`.

#### Reworked the Sojourn bot:

_The Sojourn bot no longer relies on both the entity’s health and the gauge charge. Previously, it would release the charge if the gauge was at 30 and the enemy’s health was 30. Now, the charge will only be released once it reaches 50._


#### Improvements:

Enhanced error handling for `Tws.sys`.

#### TwKernelBdridge 
Pushed the new driver `TwCmdCtrl.sys` to replace the previous `TwsCore.sys` driver.


#### Work in Progress:

1. Auto-pick for heroes.
2. MAC Address & VolumeId Spoofing.
3. CPU Spoofing Improvements.

---


### Twilight 1.9.6 - November 15, 2024
Updated to the latest game patch.

---

### Twilight 1.9.5 - November 13, 2024
Updated to the latest game patch.

Fixed high CPU usage.

### TwsMapper update 1.1 - November 10, 2024
- **Updated TwsSpoofer:**
Now supports SIMBOIOS, CPU, RAM, SSD/NVMe, BIOS, Motherboard, and Windows Product Key
### Twilight 1.9.2 Hotfix - November 04, 2024
1. Fixed Cr3 BSOD (the CR3 needed to be cached)
2. Fixed Esp / Skeleton Posation
3. Fixed Recoil
4. All functions now support `TwGetFunctionSpoofedEx`
```cpp
   TwGetFunctionSpoofedEx(void* Function, bool Function)
```

## New Recoil System (Soon Will be adjustable)

### Old recoil method
```cpp
TwWrite<float>(lEntity.GetAngle + Get->o_HorizontalRecoil, 0);
TwWrite<float>(lEntity.GetAngle + Get->o_VerticalRecoil, 0);
```

### New Recoil Method
```cpp
TwWrite<float>(lEntity.GetAngle + Get->o_HorizontalRecoil, 0);
TwWrite<float>(lEntity.GetAngle + Get->o_Recoil, 1);
TwWrite<float>(lEntity.GetAngle + Get->o_RecoilC, 0);
```
### Twilight Public launcher
1. Updated to the latest endpoint
2. Battleye APC bypass
3. EAAC Bypass

- _**Please note:** Before launching any update or bypass, we thoroughly test our products. Therefore, items 2 and 3 may take additional time to be released as we ensure they are 100% safe._

### Twilight Solutions Mapper (TwsMapper)
1. Updated to the latest endpoint 
2. Twilight drivers now load and unload as legitimate drivers
### Twilight Lua API
The Twilight Lua API is a feature we’ve been developing over the past few months. This addition allows users not only to customize settings through the Twilight GUI but also to modify the aimbot, ESP, and nearly all Twilight features. We’re continuing to refine this functionality to enhance both our product and user experience. We encourage you to explore how our Lua API works, and we’d love to hear feedback from the community to improve and tailor the API to meet user needs.

### Q & A 
#### When will it be released 
_There’s no specific release date yet, but we look forward to hearing from our community._

#### Is this feature included in the Twilight Subscription
_Yes, it’s included. However, we’re considering the possibility of offering a dedicated subscription option for developers._

#### Will it be free
_Yes, it’s completely free. However, we plan to create a marketplace to help users avoid scams from those selling fake scripts._
| Var | Type | Description |
| ------------- | ------------- | ------------- |
| b_TriggerBot   | BOOLEAN  | Enables or disables the trigger bot  |
| b_Trigger_Key   | INTEGER  | The key assigned for activating the trigger bot  |

#### Twilight C++ Function 
```cpp
void TwTriggerBot(bool enabled);
void TwTriggerKey(int key)
```
#### Adjusting BOOLEAN & INTEGER 
```lua
TwSetTriggerBot(true)
TwSetTriggerKey(0x02)
```
_You can find all keyboard and mouse virtual keys [here](https://learn.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes)._

#### Vector3 
| Var  | Type | Description |
| ------------- | ------------- | ------------- |
| v1  | Vector3  | The first vector instance for operations  |
| v2  | Vector3  | The second vector instance for operations  |
| ResultAdd  | Vector3  | Result of adding two vectors  |
| ResultSub  | Vector3  | Result of subtracting two vectors  |
| ResultScale  | Vector3  | Result of scaling a vector by a float  |
| Distance  | float  | Distance between two vectors  |

#### Twilight C++ Functions 
```cpp
Vector3 TwVector3(float x, float y, float z);
Vector3 operator+(const Vector3& V);
Vector3 operator-(const Vector3& V);
Vector3 operator*(float Scale) const;
float TwDistance(const Vector3& GetDistance) const; 
```
### Using Twilight LUA API
```lua
-- Create two vectors
local v1 = Vector3(1.0, 2.0, 3.0)
local v2 = Vector3(4.0, 5.0, 6.0)

-- Calculate the distance between the two vectors
local Distance = v1:TwDistance(v2)
```

### Example of calculating the local angle 
```lua
local lPlayer = Vector3(lEntity.lPlayer.X, lEntity.lPlayer.Y, lEntity.lPlayer.Z)  -- Local player position
local m_lEntity = Vector3(m_lEntity.X, m_lEntity.Y, m_lEntity.Z)                  -- Target enemy position

local Disctance = lPlayer:TwDistance(m_lEntity)  -- Calculate distance to the enemy


local EnemyPosition = (m_lEntity - lPlayer):Normalized() -- Scaling the direction of the vector

local TrackingSpeed = 5.0
-- Calculates the enemy's position.
-- The new angle should always use "/" instead of "*"
-- as multiplication makes the tracking too aggressive.
local CalculatedAngle = lPlayer + (EnemyPosition / TrackingSpeed)  


if Disctance < 100.0 then
    -- If target less than 100.0 trigger VK_RBUTTON
    SetKey(0x2) 
end
```
### Twilight Variables Sample
```cpp
// Primary Aimbot 
b_TriggerBot = TwSet.at("Profile").at("b_TriggerBot").TwGet<bool>(); 
b_LockOnTarget = TwSet.at("Profile").at("b_LockOnTarget").TwGet<bool>(); 
b_Prediction = TwSet.at("Profile").at("b_Prediction").TwGet<bool>(); 
b_GraviryPrediction = TwSet.at("Profile").at("b_GraviryPrediction").TwGet<bool>(); 
b_Flick = TwSet.at("Profile").at("b_Flick").TwGet<bool>(); 
b_Tracking = TwSet.at("Profile").at("b_Tracking").TwGet<bool>(); 
b_ClosestHitbox = TwSet.at("Profile").at("b_ClosestHitbox").TwGet<bool>(); 
b_DrawFov = TwSet.at("Profile").at("b_DrawFov").TwGet<bool>(); 
b_Switch_Team = TwSet.at("Profile").at("b_Switch_Team").TwGet<bool>(); 
b_PrimaryFlick_Smooth = TwSet.at("Profile").at("b_PrimaryFlick_Smooth").TwGet<float>(); 
b_Hitbox = TwSet.at("Profile").at("b_Hitbox").TwGet<float>();  
b_Aimbot_Key = TwSet.at("Profile").at("b_Aimbot_Key").TwGet<int>(); 
b_TrackingSmooth = TwSet.at("Profile").at("b_TrackingSmooth").TwGet<float>(); 
b_TrackingSpeed = TwSet.at("Profile").at("b_TrackingSpeed").TwGet<float>(); 
b_Trigger_Key = TwSet.at("Profile").at("b_Trigger_Key").TwGet<int>(); 

// Secondary Aimbot 
b_SecondaryAimbot = TwSet.at("Profile").at("b_SecondaryAimbot").TwGet<bool>(); 
b_SecondaryFlick = TwSet.at("Profile").at("b_SecondaryFlick").TwGet<bool>(); 
b_SecondaryTracking = TwSet.at("Profile").at("b_SecondaryTracking").TwGet<bool>(); 
b_SecondaryPrediction = TwSet.at("Profile").at("b_SecondaryPrediction").TwGet<bool>(); 
b_SecondaryGravity_Prediction = TwSet.at("Profile").at("b_SecondaryGravity_Prediction").TwGet<bool>(); 
b_SecondaryFlick_Smooth = TwSet.at("Profile").at("b_SecondaryFlick_Smooth").TwGet<float>(); 
b_Secondary_TrackingSpeed = TwSet.at("Profile").at("b_Secondary_TrackingSpeed").TwGet<float>(); 
b_Secondary_TrackingSmooth = TwSet.at("Profile").at("b_Secondary_TrackingSmooth").TwGet<float>(); 
b_SecondaryAimbot_Key = TwSet.at("Profile").at("b_SecondaryAimbot_Key").TwGet<int>(); 
b_SecondaryLockOnTarget = TwSet.at("Profile").at("b_SecondaryLockOnTarget").TwGet<bool>(); 
b_Secondary_SwitchTeam = TwSet.at("Profile").at("b_Secondary_SwitchTeam").TwGet<bool>(); 

```
### Known Issues for 1.9.2
NONE


