# be01
 **100 points**

This challenge is a PDF file with nested zip files.

`binwalk -Me chicken.pdf` will recursively extract all files

Layout:

chicken.pdf

└── egg.zip

    └── chicken.zip

        └── egg.zip

            └── chicken.zip

                └── egg.pdf

chicken.pdf -> egg.zip -> chicken.zip -> egg.zip -> chicken.zip -> egg.pdf

![egg.pdf]("https://i.imgur.com/7N83CBD.png")
