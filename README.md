# edge_tts_2_Colab
```python
!pip install edge-tts
```

```python
import edge_tts
import asyncio
import nest_asyncio
from IPython.display import Audio
from IPython.display import display
```

```python
import ipywidgets as widgets

nest_asyncio.apply()

# 創建文本輸入小部件
text_input = widgets.Textarea(
    value='全世界, 你們好',
    placeholder='請輸入要轉換為語音的文字',
    description='文字:',
    disabled=False,
    layout=widgets.Layout(width='500px', height='100px')
)

# 創建語音選擇下拉框
voice_selector = widgets.Dropdown(
    options=[
        ('廣東話男聲', 'zh-HK-WanLungNeural'),
        ('廣東話女聲1', 'zh-HK-HiuMaanNeural'),
        ('廣東話女聲2', 'zh-HK-HiuGaaiNeural'),
        ('日文女聲', 'ja-JP-NanamiNeural')
    ],
    value='zh-HK-WanLungNeural',
    description='語音:',
    disabled=False,
)

# 創建生成按鈕
generate_button = widgets.Button(
    description='生成語音',
    button_style='primary',
    tooltip='點擊生成語音'
)

# 創建輸出區域
output = widgets.Output()

# 定義按鈕點擊事件
async def generate_speech_from_input(text, voice):
    communicate = edge_tts.Communicate(text, voice)
    await communicate.save("output.mp3")
    return "output.mp3"

def on_button_clicked(b):
    with output:
        output.clear_output()
        print("正在生成語音...")
        loop = asyncio.get_event_loop()
        file_path = loop.run_until_complete(generate_speech_from_input(text_input.value, voice_selector.value))
        display(Audio(file_path))
        print("語音生成完成！")

# 綁定按鈕事件
generate_button.on_click(on_button_clicked)

# 顯示界面
display(text_input)
display(voice_selector)
display(generate_button)
display(output)
```
