# Edge-tts_2_Colab

```python
# 安裝 Edbe-TTD
!pip install edge-tts
```

```python
import edge_tts # 語音引擎
import asyncio # 異步處理
import nest_asyncio # Jupyter 異步問題
from IPython.display import Audio
from IPython.display import display
# 用於顯示音頻播放器
```

```python
import ipywidgets as widgets
# 用於創建互動式界面元件
nest_asyncio.apply()
# 啟用異步處理

# 創建文本輸入小部件
text_input = widgets.Textarea(
    value='這是一個文字轉語音的範例', # 預設字
    placeholder='請輸入要轉換為語音的文字', # 提示詞
    description='文字:', # 標籤
    disabled=False, # 可編輯
    layout=widgets.Layout(width='500px', height='100px') # 大小
)

# 創建語音選擇下拉框
voice_selector = widgets.Dropdown(
    options=[
        ('廣東話男聲', 'zh-HK-WanLungNeural'),
        ('廣東話女聲1', 'zh-HK-HiuMaanNeural'),
        ('廣東話女聲2', 'zh-HK-HiuGaaiNeural'),
        ('日文女聲', 'ja-JP-NanamiNeural')
    ],
    value='zh-HK-HiuMaanNeural', # 預設
    description='語音:', # 標籤
    disabled=False, # 可選
)

# 創建生成按鈕
generate_button = widgets.Button(
    description='生成語音', # 按扭文字
    button_style='primary', # 樣式
    tooltip='點擊生成語音' # 縣停提示
)

# 創建輸出區域
output = widgets.Output()

# 定義按鈕點擊事件
async def generate_speech_from_input(text, voice): # 創建物件
    communicate = edge_tts.Communicate(text, voice)
    await communicate.save("output.mp3")
    return "output.mp3" # 儲存

def on_button_clicked(b):
    with output:
        output.clear_output() # 清除
        print("正在生成語音...")
        loop = asyncio.get_event_loop()
        file_path = loop.run_until_complete(generate_speech_from_input(text_input.value, voice_selector.value))
        display(Audio(file_path))
        print("語音生成完成！")

# 綁定按鈕事件
generate_button.on_click(on_button_clicked)

# 顯示界面
display(text_input) # 顯示輸入
display(voice_selector) # 顯示語音選單
display(generate_button) # 顯示按扭
display(output) # 顯示輸出區域
```
