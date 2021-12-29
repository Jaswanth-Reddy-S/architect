
Description: General Details for Page Format
---

Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](https://www.youtube.com/channel/UCvScgo6mAvbMEjszK4sSj6g).


# Git related

* Fetch Recent files from Git
- git ls-files -z | xargs -0 -n1 -I{} -- git log -1 --format="%ai {}" {} | sort > ./output.txt (where this will store all the uploaded files name data in output.txt file)

# Azure Devops

* Script to move file to Azure Repository
```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
git config --global user.password "secret"
git checkout rollback
git add  $(Build.SourcesDirectory)/Rollback_Files/** (pushes the files to github repository)
git status
git commit -m "copy_files"
git push origin rollback
```
[Link for the same](https://stackoverflow.com/questions/66323959/how-to-move-azure-git-repos-file-from-one-folder-to-another-folder-using-azure-d)

## Adding a timestamp to a file

> new_path = '%s_%s' % (datetime.now().strftime(FORMAT), i)
> print(new_path)
>
> Using above code,time will be genearated and stored in a variable , using this variable file name can be changed to other file like a.txt to 20211227100001_a.txt

### Reading a file different formats

```python
f = open(path, "r")
while(True):
    #read next line
    line = f.readline()
```

```python
def get_last_created_file(filename):
    with open(filename) as f:
        for line in f:
            pass
    return line.split(" ")[-1]
store_file_location=get_last_created_file("./output.txt")//[2021-11-29 05:21:53 +0000 result_file.py]
```
To read in a csv format(if format is in [2021-11-29 05:21:53 +0000 result_file.py])
```python
def get_last_created_file(filename):
            a=[]
            with open(filename, newline='') as csvfile:
                spamreader = csv.reader(csvfile, delimiter=' ', quotechar='|')
                for row in spamreader:
                    a.append(row[3])
            return a
store_file_location=get_last_created_file("./output.txt") 
```

#### Checkin and Checkout in sharepoint using pnp powershell

Teamname represents project team name
say for example file is present in documents under general under 3 line is the format.(shared document constant)
Connect-PnPOnline -Url https://xxxx.sharepoint.com/sites/Teamname -Credentials (Get-Credential)
$clientContext = Get-PnPContext
$ListItem = Get-PnPFile -Url "/Shared Documents/General/filename.xlsx" -AsListItem
$targetFile = $ListItem.File
$clientContext.Load($targetFile)
$clientContext.ExecuteQuery()
$targetFile.CheckOut()
$clientContext.ExecuteQuery()
Disconnect-PnPOnline
[Link for reference](https://www.codesharepoint.com/powershell/check-in-file-in-sharepoint-using-powershell).

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
