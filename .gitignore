using BepInEx;
using BepInEx.Configuration;
using UnityEngine;
using System.Collections.Generic;
using HarmonyLib;
using GameNetcodeStuff;

namespace TelekinesisMod
{
    [BepInPlugin("com.username.telekinesis", "Telekinesis Mod", "1.0.0")]
    public class TelekinesisMod : BaseUnityPlugin
    {
        private static ConfigEntry<float> grabRange;
        private static ConfigEntry<float> throwForce;
        private static ConfigEntry<int> maxGrabbedObjects;
        private static List<GrabbableObject> grabbedObjects = new List<GrabbableObject>();
        private static PlayerControllerB localPlayer;
        private Harmony harmony;

        private void Awake()
        {
            // Configuration settings
            grabRange = Config.Bind("General",
                                  "GrabRange",
                                  5f,
                                  "The range within which you can grab objects");

            throwForce = Config.Bind("General",
                                   "ThrowForce",
                                   10f,
                                   "The force with which objects are thrown");

            maxGrabbedObjects = Config.Bind("General",
                                          "MaxGrabbedObjects",
                                          2,
                                          "Maximum number of objects that can be grabbed simultaneously");

            // Apply patches
            harmony = new Harmony("com.username.telekinesis");
            harmony.PatchAll();
        }

        [HarmonyPatch(typeof(PlayerControllerB))]
        private class PlayerControllerBPatch
        {
            [HarmonyPatch("Update")]
            [HarmonyPostfix]
            private static void UpdatePostfix(PlayerControllerB __instance)
            {
                if (__instance != localPlayer)
                {
                    localPlayer = __instance;
                }

                if (!__instance.IsOwner || __instance.inTerminalMenu || __instance.isTypingChat)
                    return;

                HandleTelekinesis(__instance);
            }
        }

        private static void HandleTelekinesis(PlayerControllerB player)
        {
            if (Input.GetKeyDown(KeyCode.E))
            {
                TryGrabObject(player);
            }

            if (Input.GetKeyDown(KeyCode.F))
            {
                ThrowObjects(player);
            }

            UpdateGrabbedObjectsPosition(player);
        }

        private static void TryGrabObject(PlayerControllerB player)
        {
            if (grabbedObjects.Count >= maxGrabbedObjects.Value)
            {
                HUDManager.Instance.DisplayTip("Telekinesis", "Cannot grab more objects!");
                return;
            }

            Ray ray = new Ray(player.gameplayCamera.transform.position, player.gameplayCamera.transform.forward);
            if (Physics.Raycast(ray, out RaycastHit hit, grabRange.Value))
            {
                GrabbableObject grabbable = hit.collider.GetComponent<GrabbableObject>();
                if (grabbable != null && !grabbable.isHeld)
                {
                    // Request ownership through the game's network system
                    grabbable.GrabObject();
                    grabbedObjects.Add(grabbable);
                }
            }
        }

        private static void ThrowObjects(PlayerControllerB player)
        {
            foreach (GrabbableObject obj in grabbedObjects)
            {
                if (obj != null)
                {
                    Vector3 throwDirection = player.gameplayCamera.transform.forward;
                    
                    // Use the game's existing throw mechanics
                    obj.ThrowObject(throwDirection * throwForce.Value);
                }
            }
            grabbedObjects.Clear();
        }

        private static void UpdateGrabbedObjectsPosition(PlayerControllerB player)
        {
            float spacing = 1f;
            for (int i = 0; i < grabbedObjects.Count; i++)
            {
                if (grabbedObjects[i] != null)
                {
                    Vector3 targetPosition = player.gameplayCamera.transform.position + 
                                          player.gameplayCamera.transform.forward * 2f + 
                                          player.gameplayCamera.transform.right * (i - ((grabbedObjects.Count - 1) / 2f)) * spacing;
                    
                    // Use the game's network-aware position system
                    grabbedObjects[i].targetPosition = targetPosition;
                    grabbedObjects[i].transform.position = Vector3.Lerp(
                        grabbedObjects[i].transform.position,
                        targetPosition,
                        Time.deltaTime * 10f
                    );
                }
            }
        }
    }
}
