# UnityAutoSceneLoader
Auto Scene Loader

```
#if UNITY_EDITOR
using UnityEditor;
using UnityEditor.SceneManagement;
using UnityEngine;

[InitializeOnLoad]
public static class AutoSceneLoader
{
    private const string scenePath = "Assets/Scenes/SampleScene.unity"; // CHANGE THIS

    static AutoSceneLoader()
    {
        EditorApplication.playModeStateChanged += OnPlayModeChanged;
    }

    private static void OnPlayModeChanged(PlayModeStateChange state)
    {
        if (state == PlayModeStateChange.ExitingEditMode)
        {
            if (EditorSceneManager.SaveCurrentModifiedScenesIfUserWantsTo())
            {
                if (System.IO.File.Exists(scenePath))
                {
                    bool shouldLoad = EditorUtility.DisplayDialog(
                          "Load Startup Scene?",
                          $"Do you want to load the scene:\n\n{scenePath}\n\nbefore entering Play Mode?",
                          "Yes", "No"
                );

                    if (!shouldLoad)
                    {
                        return; // Continue to Play Mode with current scene
                    }

                    EditorSceneManager.OpenScene(scenePath);
                }
                else
                {
                    Debug.LogError("AutoSceneLoader: Scene path not found: " + scenePath);
                    EditorApplication.isPlaying = false;
                }
            }
            else
            {
                EditorApplication.isPlaying = false;
            }
        }
    }
}
#endif
```
