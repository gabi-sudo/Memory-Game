# Memory-Game
![1](https://user-images.githubusercontent.com/82399945/131250942-6719c2e0-9005-4a2b-a9a0-dfe318dbb88a.PNG)

Windows Forms Project за предмет: Визуелно програмирање

## Опис на апликацијата

Оваа Апликација претставува едноставна игра на меморија, во која обираме во пар икони од овошје. Доколку имаме погодок иконите седат на екранот, а доколку немаме по одреден период се враќаат на празно. Играта завршува кога ке ги подогиш сите икони. 

## Упатсво

Играта е многу едноставна единствено потребно ние користење на клик за да се одкријат иконите, потоа останува на играчот да ги погоди сите икони.

## Опис на проблемот

1. За оваа игра прво треба да се размисли за изгедот. Тоа педтравува 4x4 коцки, треба да се одлучи кој елемент ке се искористе за да овозможи добра имплементација на backend функциите со што ке добиеме функционалност.

2. Следно треба да направиме функција која на "random" ке ги распоредува иконите секој пат различно, исто така фунција за кога правиме клик на иконите да знае дали имаме погодок или не.

## Решение и имплементации

1. За оваа цел прво користиме контеинер Table Layout panel во кој ставаме 4х4 Labels кои ке ни ореставуваат нашите икони.
2. Прво мора да направиме Random Object и Листа од икони кои ке ди користеми со потамошниот код.
```c#
Random random = new Random();
        List<string> icons = new List<string>()
        {
            "🍎","🍎","🍐","🍐","🍊","🍊","🍌","🍌",
            "🍓","🍓","🍒","🍒","🍑","🍑","🍍","🍍"
        };
```
Сега доделуваме случајна икона на секоја Лабела, за тоа го правиме со следниот код:
```c#
 private void AssignIconsToSquares()
        {
            foreach (Control control in tableLayoutPanel1.Controls)
            {
                Label iconlabel = control as Label;
                if (iconlabel != null)
                {
                    int randomNumber = random.Next(icons.Count);
                    iconlabel.Text = icons[randomNumber];
                    iconlabel.ForeColor = iconlabel.BackColor; //ги правиме иконите невидливи.
                    icons.RemoveAt(randomNumber);
                }
            }
        }
```
Во овај код коритиме foreach loop бидејки сакаме за секоја лабела во наштиот панел да им се стави случајна икона, за тоа го користиме Random објектот кој го направивме погоре со кој означуваме int randomNumber кој дава број од 0 до бројот на икони. Така секоја Лабела добива икона. 

После со Event Handler правиме фукнионалноста кога кликнуваме да иконите, тоа го правиме со помош на тимер бидејки после клик иконите треба да се видливи само извесен период. 
На тајмерот за  Interval Property ставаме 1000 потоа за Tick Event го писиме следниот код.
```c#
private void timer1_Tick(object sender, EventArgs e)
        {
            timer1.Stop();
            firstclicked.ForeColor = firstclicked.BackColor; 
            secondclicked.ForeColor = secondclicked.BackColor;//ги правиме иконите невидливи.
            firstclicked = null; 
            secondclicked = null; //ресетираме кликнатите икони
        }
```
 
Последно имаме функција за Кликот која кога имаме погодок ги остава видливи иконите. А доколку се разлицни иконите ги прави невидливи пак.
```c#
 private void clickImg(object sender, EventArgs e)
        {
            if (timer1.Enabled == true)
                return;
            Label clickedLabel = sender as Label;
            if (clickedLabel != null)
            {
                if (clickedLabel.ForeColor == Color.Black)
                    return;
                if (firstclicked == null)
                {
                    firstclicked = clickedLabel;
                    clickedLabel.ForeColor = Color.Black;
                    return;
                }
                secondclicked = clickedLabel;
                clickedLabel.ForeColor = Color.Black;
                winner();
                if (firstclicked.Text == secondclicked.Text)
                {
                    firstclicked = null;
                    secondclicked = null;
                    return;
                }
                timer1.Start();
            }
        }
```
Исто така функција за Победа
```c#
 private void winner()
```
Која проверува дали сите икони се одбрани и кога ке го потвди завршува играта.

## Автор

Габриела Масалковска

## Лиценци
[MIT] (https://github.com/gabi-sudo/Memory-Game/blob/main/LICENSE)
