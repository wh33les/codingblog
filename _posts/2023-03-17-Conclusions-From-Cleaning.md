---
layout: post
title: Conclusions from cleaning.  
---
This week I learned a lot when trying to clean my project data.  I fixed the loop I made with the user inputs.  At first it wouldn't change the names of the columns, even when I added the `.rename` command.  It turns out I needed to include the attribute `inplace=True`, which ensures that the data frame gets changed with the command.  I was kind of confused about that, because I think the default for that attribute is supposed to be true, but the code wouldn't work without me including it.  I also added a few more `print` commands to the loop for debugging purposes.  I'm pretty proud of the loop, I've included it all, with all the comments.
```
# Loop that will prompt me to delete or rename each column
for i in sorted(list(range(0,len(columns))), 
                reverse=True):
    # Prompt to delete
    print("Column index:",i)
    print("Column name:",columns[i])
    delete_choice = input("Delete column (y or n)?  ")
    # Make sure the input is valid
    while ((delete_choice != "y") and (delete_choice != "n")):
                          delete_choice = input("Enter y or n.  ")
    # Delete if yes
    if (delete_choice == "y"):  
                          del master_data_cleaned[columns[i]]
                          print()  
    # Prompt to rename if no
    if (delete_choice == "n"):
                          rename_choice = input("Rename the column (y or n)?  ")
                          # Make sure the input is valid
                          while ((rename_choice != "y") and (rename_choice != "n")):
                              rename_choice = input("Enter y or n.  ")
                          # Rename if yes
                          if (rename_choice == "y"):
                                new_name = input("Enter the new name: ")
                                master_data_cleaned.rename(columns = {columns[i]:new_name}, inplace=True)
                                # Verify the name was changed
                                new_columns = master_data_cleaned.columns.values.tolist()
                                master_index = new_columns.index(new_name)
                                print("Now new_columns["+str(master_index)+"] = "+new_columns[master_index]+".")
                                print()
                          # Pass if no
                          if (rename_choice == "n"):
                                print()
                                pass 
```
Once I got the loop completely working, I started to go through it and edit each of the 198 columns, but once I did I started to think this really isn't the most efficient way to clean this data, maybe there's still a better way.  I also noticed that even with the English translations, many of the column titles were still not descriptive enough for me to use in data analysis, so it felt like I was throwing out a lot of data.  

Matt Osbourne, who runs the data science bootcamp, had office hours on Wednesday so I went and asked him for feedback.  He said maybe the reason the translations weren't good was because the column names had numbers at the beginning and underscores instead of spaces.  He suggested I get rid of the numbers and underscores and then try translating it.  I didn't know the code that would do those things, but when I worked on it yesterday I discovered it was pretty easy to find.  Then I thought Google translate itself might be the problem, so I searched for better alternatives and found DeepL.  

I read on one webpage that DeepL consistently gets better reviews than Google trnaslate for its accuracy.  DeepL has a python wrapper but it requires an authentication key.  Here was something else I learned how to do.  I remembered Matt using an API key in one of the bootcamp lectures so I went back to the lecture notes to see how he did it.  Following his notes, I created a `.py` file with a function that returns the key, then called and used the function in my code.  I assumed it would be a good idea to hide my `.py` file, since the free version of DeepL only allows for translation of 500,000 characters per month.  So I figured out how to create a `.gitignore` file to keep the `.py` file out of my public remote repository.

The second round of translation did give better results than the first time, but I still found many of the columns in the data not descriptive enough.  I expressed this concern at Matt's office hours today and he helped me try to find a publication accompanying the data that might give more information.  We looked up the DOI and still only came up with a page on [Zenodo](https://zenodo.org/record/4324025#.ZBTyrXbMK3D) with just a summary of the data.  More and more I was feeling like this was just a bad data set, and eventually I decided it would be best to just choose a different data set and start over with the project.

So now I'm back at square one.  But I just started my spring break, so we'll see what kind of progress I make during this time.
