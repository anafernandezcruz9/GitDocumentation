# Git Documentation

This tutorial is based on a series of videos where everything we are going to go over in this summary is covered in more depth and detail. This is the link of the first video of the series. 

## How Git works?

### Data Base and Tree Structure. Git acts as both a database and a tree structure.

    Regarding the tree structure: the leaves of the branches are the files we will create or update and the branches will be the different directory structures.

    Regarding the database part: it stores 4 different types of objects. We can access them through their key. There is only one type of key: cryptographic hashes (40 digits - SHA1). Git way of working: each version of the file is converted into binary and linked to a hash. The complete object is stored with some additional metadata..Therefore the four objects we find in the database are:

    - Blobs: corresponds to files or versions of files that are going to be the leaves of our tree
    - Directory structure - trees
    - Commits or changes related to files or leaves: each commit has a sha. A commit is a reference.
    These three objects are immutable, that is, they persist data structures.
    - Tags: tags contain references to commits, commits contain references to trees and trees contain references to blobs.

    Commands to access the database: ( **** refers to hash abbreviation)
    - git log --pretty=oneline shows us a list of the latest commits.
    - git show **** (almost 4 initial commit hash numbers) shows a summary of that commit.
    - git log --pretty=oneline --abbrev-commit is equal to  git log --oneline and  opens hashes to less digits
    - git ls-tree **** (commit numbers): gives us the order of the blobs and the hash associated with each blob.
    - git show hashblob****: shows the content of the blob, i.e. the file.
    - git cat-file -t ****: shows the information of the hash, i.e. refers to a blob, tree..

    ! Special to note: When we create a substructure of folders (and therefore touch the tree structure) it is not necessary to commit the changes because there is nothing to commit.The moment we add a file and do git status and then do git add to add the file and do git status again we see that only a new file appears, nothing else, it doesn't say anything about the structure. If we do a commit of the changes and then a git show we see the information of the last commit (in Figure 1, last commit is 8b31).

    If we do git ls-tree hashcommit here it gives us information about the blob we have added (but that is because we are launching the commands from the subdirectory).

    If we change to the directory above and launch the last command (exactly the same) we see that it no longer gives information about the blob but talks about tree with the name of the folder containing the file.
    
    <img width="483" alt="1" src="https://user-images.githubusercontent.com/75982999/182707463-20f5dc11-e242-4378-9c64-b745a69ffb16.png">

    Figure 1. Examples of blob and trees hashes listing tree structure in different subdirectories.

### Pointers and Tree as models. 

    If we study Git as a model tree we see that each level of the branch structure is composed of a key (hash) and pointer pair where the pointer points to the next level.

    In list A we see that the first pointer points to a second key and pointer pair. The second pointer points to a third last object which is the single object or the leaf of the tree. The leaves are not pointers, since they do not point anywhere and there is no identifying hash of that pointer.

    List B is composed of the same elements as list A, only with a pointer pointing to list A. The fact of working with pointers makes it NOT necessary to copy the elements in A but the pointer points to the list.

    This saves memory and increases speed.

    This is possible because pointers are inmutables.

    The elements of the list are shared if we prepend the list. If append is to add an element to the end of the list which means rebuilding the list, prepend adds the element to the beginning and the list remains unchanged.

    <img width="586" alt="image" src="https://user-images.githubusercontent.com/75982999/182707549-ff6333f1-b3ce-41d4-8c54-28f1b93dcc44.png"> 

    Figure 2. How pointers work

    Git works with Parent Reference. In Figure 3 for shorting, the circle is the pointer and the abc* is the hash number. This is an example where the schema starts with the initial state or initial commit. In Figure 3 we can see that from the Inital State there are two different hashes that points to it. This two hashes means that are two paths of changes over the initial state. The changes continue evolving by doing another change to the hash abc1 but also to hash abc4. The final state is the union of both paths and it’s called merge commit  (merge of both paths).The most important thing in the image is the direction of the arrows. If we look at the time axis, the newest changes appear on the right but the pointer arrows point to the left. This is because of the Parent Reference since the pointers point to the previous state from which they come in order to redo the history of a document.
    
    <img width="659" alt="image" src="https://user-images.githubusercontent.com/75982999/182707617-e8753f23-8e3d-4b20-812c-58f88573d0de.png">

    Figure 3. Parent Reference

## The branching model

### Nomenclature fundamentals: in git we will use the following terms all the time so it is important to underline them and always have them at hand.

    1. Main: default branch that is automatically created when a repo is created (before 2020 it was named after master). It is considered the main branch to develop our project. 
    2. Origin: main remote repository
    3. Head: refers to the commit where my repository is located - where we work.

    We differentiate between the local head and origin/head.

### How the branching model works?
