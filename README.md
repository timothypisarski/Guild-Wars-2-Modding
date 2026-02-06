# Guild-Wars-2-Modding
Modding resources related to modding Guild Wars 2 with the 3DMigoto | SilentNightSound/GI-Model-Importer tools

## Tools
 - GI-Model-Importer: https://github.com/SilentNightSound/GI-Model-Importer/releases
 - Blender (no higher than 3.6.1) with Blender plugin blender_3dmigoto_gimi.py from above repository

## Disclaimer
This is by no means a comprehensive, fool proof guide on how to mod Guild Wars 2. 
<br><br>
However, these are things i've learned over the last several years and how *my* process works. 
<br><br>
While I make no promises about being banned, I have run these mods for at least 4 or more years and have never had any issues. I would not expect this to cause any issues as it is client-side only. So any risk to your account is minimal in my mind. 
<br><br>
That being said, if you go on official Arenanet channels and promote moddding then you are probably asking for trouble at that point.

## What is possible | What is not

**We can** create a custom mesh with custom textures and display that in place of a similar mesh in the game. Realistically you could turn NPCs into bananas if you really wanted. <br>
**We can** display our mods on the Client-side ONLY. 
<br>
<br>
**We cannot** do Skyrim level modding. We can't add new items, modify skeletons, or animations or anything really fancy. <br>
**We cannot** make our mods visible to other players.

What this does is essentially display Model A instead of Model B. In some scenarios we can display Model A and Model B if we wanted. 


## Steps
1)	Unzip the contents of either 3dmigoto-GIMI-for-development.zip OR 3dmigoto-GIMI-for-playing-mods.zip into your main Guild Wars 2 directory<br>
	1)	You’ll need the Development version if you want to extract models and create mods
2)	Modify the d3dx.ini to at least set the target to your Guild Wars 2 executable name (Gw2-64.exe) e.g.: target = Gw2-64.exe<br>
   	1)	There are some other settings that you should ensure are changed but review the d3dx.ini file in this repository
3)	Start Guild Wars 2, if its working (and you are using the Development version) you should see the Green text on the top and bottom of the screen<br>
   	1)	numpad 0 to hide the text for playing<br>
	2)	Hiding it will also disable the controls for doing a frame analysis, searching buffers, etc.
4)	If you need to find and export a piece of armor:<br>
   	1)	Go somewhere where you will be the only character<br>
	2)	This makes it easier to search through available buffers
5)	Ensure the Green text is displayed to be in hunting mode
6)	See below to cycle through shaders (we usually want VB)<br>
   	1)	Pressing / and * on the numpad cycles through the Vertex Buffers (VB), which contain information on the vertices for the objects being drawn to the screen. You can copy the hash of the currently selected VB with numpad - <br>
	2)	Pressing 7 and 8 on the numpad cycles through the Index Buffers (IB), which contain information on how vertices are connected to form the model. You can copy the hash with numpad 9 <br>
	3)	Pressing 4 and 5 on the numpad cycles through the Vertex Shaders (VS), which contain information on how vertices/faces are positioned on the screen. Use numpad 6 to copy the hash <br>
	4)	Pressing 1 and 2 on the numpad cycles throught the Pixel Shaders (PS), which contain information on how textures and colors are applied to the objects. Use numpad 3 to copy the hash <br>

7)	Copy the hash using the numpad key next to the keys in the same row <br>
	1)	Like the  - key for / and * <br>
	2)	You will need this for the .ini file later <br>
8)	F8 to dump the screen <br>
	1)	This can take a while and will be MASSIVE in size if you are dumping everything <br>
	2)	IT IS HIGHLY RECOMMENDED, to create a mod that will ONLY dump the hashes you are looking for. <br>
	   	1)	Create a separate folder in your Mods folder. (i.e. C:\Guild Wars 2\Mods\hashdump) <br>
		2)	Create an .ini in your new folder and enter lines like this to ONLY dump specific hashes: <br>
		3) See my ModelExtract mod folder for ini example <br>
9)	This should export ALL models, textures, etc.
10)	Make sure you copied the VB, PS, or VS and then search in Windows for this or in Blender when you import the frame analysis dump
	1)	There will be several files of the same size with similar names, you only need to import one of them in Blender, they are all the same.
11)	Using the Blender plugin, import the VB files you need using the hash of the VB
	1)	When you import into Blender you may have multiple meshes that are all the same. You only need one of these meshes. 
	2)	Delete the rest.
12)	Modify, add, remove what you need, ensure you add the textures in Blender so you can see how they "should" look in game
13)	Export via the Blender plugin
14)	Create a separate folder for just these exported pieces in the Mods folder
15)	Place all the exported files in that newly created folder
	1)	so directory would be like ../Mods/myNewModName
16)	Create an .ini file in the folder
17)	Modify the .ini to point to the appropriate pieces and textures
	1)	see any of the .ini files and you will get an idea of how this works
		(WILL WRITE SOMETHING MORE COMPREHENSIVE LATER)

## FAQS

**Question: Why when i transmog just my chest to something different does my modded armor disappear?** <br>
**Answer:** Sometimes a combination of armor pieces make up a single hash. So if you have a specific chest/gloves equipped then thats a specific hash. This is the hash you set in your .ini file. When you change either the chest or gloves, it changes the hash and thus removes your mod since the mod only replaces that specific hash.
<br>
<br>
**Question: When i do a frame analysis dump (F8) my resulting folder is empty!** <br>
**Answer:** Check your d3dx.ini and make sure this line is not commented out and looks like this: <br><br>
`analyse_options = dump_rt dump_tex dump_cb dump_vb dump_ib buf txt` <br><br>
Although, it is HIGHLY RECOMMENDED to leave this commented out and create a separate mod folder to only dump specific hashes.
<br>
<br>
**Question: My model is completely broken in game. Vertices are stretching into infinity. Why?**
<br>
**Answer:** Could be multiple reasons. Some common reasons I've run into: 
1) Your stride set in your .ini file doesn't match the stride of the mesh you are replacing
2) Your stride set in Blender on the Object properties doesn't match the stride of the mesh you are replacing
3) Your format set in your .ini file doesn't match the format of the mesh you are replacing
4) Your format set in Blender on the Object properties doesn't match the format of the mesh you are replacing

**Question: I want to add a custom beard or hair but can't find the hash to replace only the beard or hair. What gives?** <br>
**Answer:** Not all but **most** of the beards (and hair for that matter) are drawn together with several other pieces of the environment. This means that you will not be able to replace only the beard/hair. It also means that when doing a Frame Analysis it will create a massive folder (in terms of size) as it dumps tons of textures and models. <br>
One way I was able to somewhat get around this is find a hair or beard model that has its own hash, meaning the hash only contains the beard/hair, and replace that instead. It is less than ideal but we are quite limited with what we are able to do.

**Question: I want a deeper dive on how to create mods.** <br>
**Answer:** There are alot of great guides in SilentNightSound's Github repository. Look at the Guide section and read through those. They are specific to Genshin Impact but most of the principals can also be applied to modding Guild Wars 2. See here: [https://github.com/SilentNightSound/GI-Model-Importer/releases](https://github.com/SilentNightSound/GI-Model-Importer/releases)


## PREVIEWS

Imported a weapon from Final Fantasy XIV
<img width="784" height="1276" alt="screenshot1" src="https://github.com/user-attachments/assets/f11ff6d7-b9e7-4b72-9be5-04574ff4ab56" />

Kitbash of heavy/medium/light armors
![screenshot2](https://github.com/user-attachments/assets/6c868b98-d65d-4486-a96f-31086958be8a)

Resized and replaced weapon model (Norn Greatsword over Shimmering Greatsword)
![screenshot3](https://github.com/user-attachments/assets/9e97e217-b9ef-41ba-a37a-37541cb66db6)

Another kitbash of heavy/medium/light armors with upscaled textures
![screenshot4](https://github.com/user-attachments/assets/b3b1d97b-8f69-4ffe-8043-1e399d5c8f0b)



