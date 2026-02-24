# Heading Level 1

#### Heading Level 4

### 2. Duplicate the following paragraph. Pay attention to the bold, italic, blockquotes, numbered lists, and unordered list items.

I just love **bold_text**, _italic_text_ and **_bold_italic_text_**. Just look at the following blockquotes:

> This blockquote is very helpful
> 
> > Alongside this sub blockquote

I also like the folloing numbered list items:

1. List 1
2. List 2 a. Nested list
    - With an unordered sublist

Let's end this with three horizontal lines:

---

---

---

### 3. Create this link "[https://www.dataquest.io/blog/jupyter-notebook-tutorial/](https://www.dataquest.io/blog/jupyter-notebook-tutorial/) "as clickable with this title "How to Use Jupyter Notebook"

This is a [tutorial link](https://www.dataquest.io/blog/jupyter-notebook-tutorial/ "How to Use Jupyter Notebook")

### 4. display the following image from this link: [https://kiran-parte.github.io/aiforall/assets/images/blog/jupyter.jpg](https://kiran-parte.github.io/aiforall/assets/images/blog/jupyter.jpg)

![image](https://kiran-parte.github.io/aiforall/assets/images/blog/jupyter.jpg)

### 5. Create the following table

| Column 1 | Column 2 | Column 3 |
| -------- | -------- | -------- |
| Row 1    | Row 1    | Row 1    |
| Row 2    | Row 2    | Row 2    |
| Row 3    | Row 3    | Row 3    |

### 6. Write the followng formulas in a new cell (markdown cell):

#### Relevant formulas

- square area: s=(2r)2
- circle area: c=πr2
- c/s=(πr2)/(4r2)=π/4
- π=4∗c/s

### 7. Embed the following python code

Python code for addition

a = 10
b = 10
c = a+b
print(C)

### 8. Generate a square shape like the circle shape below using Scalable Vector Graphics in cell magic

%%SVG

<svg weight="100" height="100">
    <circle cx="50" cy="50" r="40" stroke="blue" stroke-weight="4" fill="brown"/>
</svg>

### 9. Show how much time this program spent in each function with the cell magic command of pyhton code profiler

def function():
    i = 1000
    sum = 0
    while i > 0 :
        i = i - 1
        sum = sum + i
    print(sum)