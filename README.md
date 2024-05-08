# easy_mode_LLM_inference 
硬件环境：  
apple mac M1  
python 3.10  

LLM推理最大的问题在于相应时间，过多的延迟导致用户体验降低  

如何结果这个问题，参考大佬的解决办法就是通过llama.cpp(纯c++、C)  
（refer to https://github.com/ggerganov/llama.cpp)  
简单的LLama.cpp使用

1 首先下载llama.cpp  
git clone https://github.com/ggerganov/llama.cpp

2 编译  
cd llama.cpp  
make  
生成./main（用于推理）和./quantize（用于量化）二进制文件  

gpu版  
macOS用户无需额外操作，llama.cpp已对ARM NEON做优化，并且已自动启用BLAS。M系列芯片推荐使用Metal启用GPU推理，显著提升速度。只需将编译命令改为:  
LLAMA_METAL=1 make  

3  下载模型  
中文混合模型，可以直接下载gguf格式。下载ggml-model-q4_0.gguf，gguf意味着量化后的int4结果。量化大大降低了模型大小，7b的模型大概在4GB上下。  
链接：https://huggingface.co/hfl/chinese-llama-2-7b-gguf/tree/main  
下载后把文件放在llama.cpp下面的models/7B文件夹下  

4 推理  
在llama.cpp下运行  
./main -m ./models/7B/ggml-model-q4_0.gguf \  
        -t 8 \  
        -n 128 \  
        -p 'The first president of the USA was '  


-p 表示提示句  
-temp 温度  
-top-k top-k sampling  
-t 线程的使用情况  
-i 交互模式

5 结果  
what is the national flower of the USA? 美国的国花是什么 答：美国没有官方国花，不过美国的州旗上都有各自的州花。是指挥官花，在南方。美国的国花是红花木莲。红花木莲是北美洲热带的多年生灌木植物，原产于墨西哥南部、中美洲和加勒比海诸岛。红花木莲是红花木莲属（木莲属）植物中的一种。它... 问： 国花是什么 答：国花：木棉花 问： 国花是什么花？答：国花  
total time =    9880.40 ms /   135 tokens



