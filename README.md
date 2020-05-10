# Timio
## Background
Timio is an educational audio and music player for kids. It consists of a battery powered player and 20 disks with content such as nursery rhymes, games and lullabies. The content is provided in 6 languages. English, Spanish, French, German, Dutch, Chinese. More info is available at [timio.co](https://timio.co/).

To change the language you need to remove the disk and hold the language selection button. This is great until you realise it's the first thing a toddler will do. So my son ends up listening to nursery rhymes in a language he has no idea about and loses interest in is new toy. I wondered if I could modify the Timio to only made some languages available.

## SD Card
Content is not actually stored on the disks. Swapping the disk just changes the content selection stored on the device. There is an SD card in the battery compartment containing the content. The instructions make it clear to not remove or modify so obviously I pulled it out and stuck it in a computer. This is what I found.

* The card provided is a Sandisk Class 4 8GB card. 
* The content is only around 170MB.
* Filesystem is FAT32
* All files are .a18. I've had no luck identifying the format.

## Directory structure
At the top level there at 7 directories:
```
L00
L01
L02
L03
L04
L05
L06
```

### L00
L00 seems to contain some common files:
```
YE.a18
Disc.a18
L0009S01.a18
L0009S02.a18
L0009S03.a18
L0009S04.a18
L0009S05.a18
L0009S06.a18
L0009S07.a18
L0009S08.a18
L0009S09.a18
L0009S10.a18
L0009S11.a18
L0009S12.a18
WEL.a18
```

### L0X
The L0X directories are the language specific files. They contain a load of files named using the following standard:

```
LXXYYYYY.a18
XX      Language ID
YYYYY   Content ID
```

e.g. E:\L01\L0100C01.a18 is for language 01 (English) content 00C01.

These are the language IDs:
```
01 - English
02 - Spanish
03 - French
04 - German
05 - Dutch
06 - Chinese
```

## Content
When changing the language the, the Timio will announce the one selected. I assumed this came from the L00 directory, but after ~~breaking~~ modifying the directory structure, it seems to come from one of the LXXYYYYY.a18 files.

All languages contain the content ID files named using their language ID.

## Modifying Languages
My aim was to remove the languages that I don't want to use that the moment so that's where I started.

*Take a backup before doing anything and do not modify the included card* I doubt the manufacturer will be happy when you ask for a replacement.
I backed up my files and used a new SD card formatted for FAT32 for testing. I initially copied all the files over and tested it still worked in the Timio.

## Removing Languages
Since the file naming has been identified, we can replace the languages we don't want, with one we do. In these cases I kept the L00 directory untouched.

### To replace all with English
With SD mounted to E:\ and backup of the Timio files in C:\temp\timio

```powershell
foreach ($id in (2..6)) {
    mkdir "e:\L0$id\";
    Get-ChildItem C:\temp\timio\L01\ | %{
        cp  $_.fullname  "e:\L0$id\$($_.name -replace '^L01', "L0$id")"
    }
}
```
When changing language now the Timio will only use English.

### To replace with only English and Spanish
In this case I replace alternate languages with English and Spanish. This way cycling through the languages with switch between the 2.
```powershell
foreach ($id in (1, 3, 5)) {
    mkdir "e:\L0$id\";
    Get-ChildItem C:\temp\timio\L01\ | %{
        cp  $_.fullname "e:\L0$id\$($_.name -replace '^L01', "L0$id")"
    }
}

foreach ($id in (2, 4, 6)) {
    mkdir "e:\L0$id\";
    Get-ChildItem C:\temp\timio\L02\ | %{
        cp  $_.fullname "e:\L0$id\$($_.name -replace '^L02', "L0$id")"
    }
}
```

## Next Steps
* I have no idea what format the .a18 are. I'd like to find this out and see if content can be modified.
* I'd like work out how the content ID is linked to the disks. 
* Can L0X directories be removed or added?