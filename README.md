# 🧰 UCDC Mod Toolset

Welcome! This project is a Mod Toolset for the game Unity-Chan: Desktop Companion. Its goal is to make modding the game smooth and easy.

If you have any suggestion or found any issues, please join our Discord Server, we have a section to showcase your mod here too!

---

## 📦 Requirements

- Unity **2023.2.20f1+** (LTS recommended)
---

## 🔨 Installation - Manual

1. Create an empty Unity project.
2. Download the latest `.unitypackage` from the [Releases tab](https://github.com/JacopoDev/UCDC-mod-toolset/releases).
3. Drag the `UCDC Mod Toolset.unitypackage` to your Unity project window.
4. Click `Import` and confirm that you want to import all files into your project.


## 🚀 Installation - Unity Package Manager

1. Create an empty Unity project.
2. Download the latest `.unitypackage` from the [Releases tab](https://github.com/JacopoDev/UCDC-mod-toolset/releases).
3. On the top bar go to **Window → Package Manager**.
4. Click **+** and **download package from Git URL**.
5. Enter `https://github.com/JacopoDev/UCDC-mod-toolset.git`
6. Click `Import` and confirm that you want to import all files into your project.

---

## 🛠️ Creating Your First Mod

1. Do on of the Installation steps as described above.
2. On the top bar go to **Mod Tools → Create → New Mod**
3. It will ask for the name of the mod, and will create a directory for this mod in the `Assets` folder
4. Add a new script in this new folder, for testing purposes add this code inside:
```
using System;
using System.Net;
using System.Threading.Tasks;
using UCDC_Mod_Api.GameInterfaces;
using UCDC_Mod_Api.Models.TextGen;
using UCDC_Mod_Api.ModInterfaces;
using UMod;

namespace Hello_World_Mod
{
    public class HelloWorld : ModScript, ITextAiAccessor
    {
        // game will call it to provide its API database to the mod
        public void SetProvider(IAiApiProvider database)
        {
            database.SetActiveTextAccessor(this); // Makes our script control the text generation
        }
        
        // a method that game will use to generate message, it is called as an async Task
        public int GenerateMessage(IChatProvider aiProcessor, Action<TextResult> finishedAction)
        {
            int result = SendAfterTime(finishedAction).Result;
            return result;
        }
    
        private async Task<int> SendAfterTime(Action<TextResult> finishedAction)
        {
            await Task.Delay(500); // Waits for 0.5 seconds - emulate thinking, giving time for animations
        
            TextResult result = new TextResult()
            {
                Code = (int)HttpStatusCode.OK, // this will tell the game that generation went without errors
                Message = new Message()
                {
                    role = "assistant", // possible values - system, assistant, user
                    content = "Hello World!" 
                }
            };
        
            finishedAction.Invoke(result);
            return result.Code;
        }
    }
}
```
5. On the top bar go to **Mod Tools → Build Mod**
6. Copy the resulting `.uccdcmod` file to the `%USERPROFILE%/.unityChanCompanion/Mods/` folder.
7. Launch the game, from now on Unity-chan should repeat only one phrase - "Hello, World!"


---

## 📁 Template Mods

Need examples? Check out the template mods here:

👉 [Template UCDC Mods](https://github.com/JacopoDev/ExampleMods)

Includes:
- Basic model override mod
- More advanced moder override (AI prompt, ragdoll, face expressions, headpats support) mod
- Text Generation override mods
- Voice Generation mods

---

## 🧾 License

This toolset is provided under the [MIT License](LICENSE).  
You are free to use, modify, and distribute it.  
No warranty is provided.

---