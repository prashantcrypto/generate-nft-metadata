# generate-nft-metadata

Simple toolset to generate metadata.

## Support
Like this tool? Want to support me? Please donate some crypto to me (so I can support my 4 wives and 11 kids and 3 dogs) 

** ETH : 0xb0e73af58a9fdece76ba74a0cfb09265ae7e45d0

** Polygon : 0x70a0D3c75853f706B17970727A25113a63bCAf1f

## Art Engine
FYI , I have forked the HashLips Art Engine and made some extra features on it

https://github.com/fransyozef/hashlips_art_engine

Go check it out, maybe it can help you also.


## Index

- Setup
    - [Download](#download)
    - [Window users](#windows-users--use-gitbash-terminal-for-windows)
    - [NodeJS](#nodejs-version)
    - [Install](#install)
    - [Config.js](#config-file)
- The tools
    - [Tool 1 : Generate json file from images](#tool-1---generate-json-file-from-images)
    - [Tool 2 : Generate a collection with only 1 media](#tool-2---generate-a-collection-with-only-1-media)
    - [Tool 3 : Generate json files from the _metadata.json](#tool-3---generate-json-files-from-the-_metadatajson)
    - [Tool 4 : Update the base uri](#tool-4---update-the-base-uri)
    - [Tool 5 : Reverse generate images from _metadata.json and layers](#tool-5---reverse-generate-images-from-_metadatajson-and-layers)
    - [Tool 6 : Cleanup metadata](#tool-6---cleanup-metadata)



## Download 

You can download the latest release at https://github.com/fransyozef/generate-nft-metadata/releases

or you can clone this on your machine :

```sh
git clone https://github.com/fransyozef/generate-nft-metadata.git
```

## Windows users !!!! Use GitBash terminal for Windows
Some commands need some unix commands. The Windows terminal don't support them. In order to make it work you could use Git Bash as your terminal (as a replacement of PowerShell). Download it at https://gitforwindows.org/.

So use Git Bash instead of Windows PowerShell!

Without this, the scripts won't work!

## NodeJS version
This tool was tested with nodejs version v14.19.1. You need to download and install it from https://nodejs.org/en/download/.

## Install
Open your terminal and navigate to the project root folder and type :

```sh
npm install
```

## Config file

The config file is located in `/src/config.js`. This is where you configure the tools. 


## Tool #1 - Generate json file from images

Let's say you have some images, but don't have any metadata json files.

Put your images in the `/assets/` folder.

Next you can setup the default metadata that will be used in your metadata json. 
You can find this in the `config.js` file.

```
    namePrefix: 'Your Collection',
    description: 'Remember to replace this description',
    baseUri: "ipfs://NewUriToReplace/",
```


You can configure the attributes and extra metadata in the `config.js` file :

```
    extraMetadata : {
        author : "Fransjo Leihitu"
    },
    extraAttributes : [
        {
            "trait_type": "color",
            "value": "black"
        }
    ],
```

then run the command 

```sh
npm run generate:fromImages
```

this will generate the json files in `/build/json` and rename & copy your source images files to `/build/images/` folder.

So it will look something like this

```
{
  "name": "Your Collection #1",
  "description": "Remember to replace this description",
  "image": "ipfs://NewUriToReplace/1.png",
  "dna": "55d323fe03e5f28360dbf1b75f1a87ef09d0b6a2",
  "edition": 1,
  "date": 1651319815313,
  "compiler": "Metadata generator NFT by fransyozef",
  "attributes": [
    {
      "trait_type": "color",
      "value": "black"
    }
  ],
  "author": "Fransjo Leihitu"
}
````


## Tool #2 - Generate a collection with only 1 media

If you want to generate metadata for your collection witht only 1 media (png,mp4) , you can use the command

```sh
npm run generate:fixedMedia
```

In the config file you can set the filename and the editions to generate :

```
    fixedMedia : {
        filename : 'test.png',
        editions : 10
    }
```

Don't forget to update the following in your `config.js`

```
    namePrefix: 'Your Collection',
    description: 'Remember to replace this description',
    baseUri: "ipfs://NewUriToReplace/",
```

This will generate 10 metadata with all pointing to the same filename in the folder `/build/json/`

For example:

```
{
  "name": "Your Collection #1",
  "description": "Remember to replace this description",
  "image": "ipfs://NewUriToReplace/test.png",
  "dna": "f24ec0656aa4e8706480d46b7655fd07e9d293fe",
  "edition": 1,
  "date": 1651600070254,
  "compiler": "Metadata generator NFT by fransyozef",
  "attributes": [
    {
      "trait_type": "color",
      "value": "black"
    }
  ],
  "author": "Fransjo Leihitu"
}
```


## Tool #3 - Generate json files from the _metadata.json

If you want to generate json files that are stored in the _metadata.json, use the command

```sh
npm run generate:fromMetadata
```

Put the `_metadata.json` in the `/assets/` folder. The json files will be exported in the `/build/json/` folder.

So for example :

```
[
    {
        "name": "Test #1",
        "description": "My description",
        "image": "ipfs://NewUriToReplace/1.png",
        "dna": "53253828be96096cb1a321e5a3bce241db6c067f",
        "edition": 1,
        "date": 1651315338049,
        "author": "Fransjo Leihitu aka The Hangry Coder aka XperimentalConcept aka fransYOzef aka ThaLyric",
        "attributes": [],
        "compiler": "HashLips Art Engine , forked by fransyozef"
    },
    {
        "name": "Test #2",
        "description": "My description",
        "image": "ipfs://NewUriToReplace/2.png",
        "dna": "a9ab967c5e138cfb743a0d477aa4f827672eac99",
        "edition": 2,
        "date": 1651315339045,
        "author": "Fransjo Leihitu aka The Hangry Coder aka XperimentalConcept aka fransYOzef aka ThaLyric",
        "attributes": [],
        "compiler": "HashLips Art Engine , forked by fransyozef"
    }
]
```

will generate 2 json metadata files in `/build/json/`.


## Tool #4 - Update the base uri

Once you have uploaded all your images to Pinata / IPFS , you need to update the baseUri in the json files.

Open the config file in `src/config.js`  and change the `baseUri` value. 

Next, in your terminal type

```sh
npm run updateBaseUri
```

## Tool #5 - Reverse generate images from _metadata.json and layers

This tool will generate your images by crossreferencing the `_metadata.json` and the source image in `/assets/layers/`.

The images will be outputtted in `/build/images/`.

First set the correct width and height output in the `config.js`

```
    format: {
        width: 512,
        height: 512,
        smoothing: false,
    }
```

The command to run this tool :

```sh
npm run generate:fromMetadataJsonAndLayers
```

## Tool #6 - Cleanup metadata

The HashLip generator creates metadata for example

```json
{
  "name": "Your Collection #1",
  "description": "Remember to replace this description",
  "image": "ipfs://NewUriToReplace/1.png",
  "dna": "ffca094d38469d558fd8a442b847802d3ef3b42e",
  "edition": 1,
  "date": 1651601900137,
  "compiler": "Metadata generator NFT by fransyozef",
  "attributes": [
    {
      "trait_type": "color",
      "value": "black"
    }
  ],
  "author": "Fransjo Leihitu"
}
```

You don't actually need all the keys. Tool 6 will delete these :

- dna
- edition
- compiler
- author
- date

Put all your metadata in the folder `/build/json/` .

Then run the command in your terminal

```sh
npm run cleanMetadata
```