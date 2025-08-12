# Making a model Transformers.js Compatible

Previously, The only models which could be used within the system were those found on hugging face which were already made compatible.

This limited the potential models, and prevented the usage of custom or more powerful models. To fix this issue, the processes for converting a model to become transformers.js compatible has been found, with the instructions to do so listed below.

The process shown below only works for pytorch models, and further research is needed to determine the process for converting other model types.

Prerequisites:
- Install Optimum via `python -m pip install optimum[onnxruntime]`

Process:
1. Clone the repository of the pytorch model from hugging face. You can remove the ReadMe and git files.

2. Ensure the main pytorch_model.bin file was downloaded, and if not download that file separately and place it in the same location as the other files.

3. Create two temporary folders called `Exported` and `Quantized`. The exact name doesnt matter, and these folders can be deleted after the conversion process is completed, but these names are used for later steps.

4. Next, run this command in a terminal.
`python -m optimum.exporters.onnx --model <Downloaded Model Folder Path> <Exported Folder Path> --task <Model task>`
In this command, substitute in your file paths to the correct folders, and the model task with the correct variable, such as `summarization` or `question-answering`. The fill list can be found [here](https://huggingface.co/docs/transformers.js/en/index).

5. Create a python file with the following contents:
    ```py
    from optimum.onnxruntime import ORTQuantizer
    from optimum.onnxruntime.configuration import AutoQuantizationConfig

    dqconfig = AutoQuantizationConfig.avx512_vnni(is_static=False, per_channel=False)

    quantizer = ORTQuantizer.from_pretrained("Exported", "decoder_model.onnx")
    quantizer.quantize(save_dir="Quantized",quantization_config=dqconfig)

    quantizer = ORTQuantizer.from_pretrained("Exported", "encoder_model.onnx")
    quantizer.quantize(save_dir="Quantized",quantization_config=dqconfig)
    ```
    This python file will quantize the model encoding and decoding, which will reduce the models size without impacting performance or accuracy. More information about this can be found [here](https://huggingface.co/docs/optimum/en/onnxruntime/usage_guides/quantization).

6. Run the python file. Inside the `Quantized` folder you should find two onnx models, one called `decoder_model_quantized.onnx`, and the other called `encoder_model_quantized.onnx`.

7. Rename the `decoder_model_quantized.onnx` to `decoder_model_merged_quantized.onnx`. This will not change the model in any way but will let transformers.js recognise it.

8. Copy the two onnx models back into the original model repository file. You can delete the pytorch_model.bin file. This is to allow the model to still use the original tokenizer, as otherwise the model will product malformed 
gibberish.

9. Finally, create a new file called `browser_config.json`. Inside this file add the text `{"fileName":"<Model/Name>"}`, but replace the model name with the same name found at the original model repository on hugging face.

Now, this new model should be able to be uploaded and run on the Local Chat browser. The `Exported` and `Quantized` folders and their contents can be deleted now, since they are no needed.

Further research into how other model types can be converted and used in our system will be conducted in later sprints.
