# Version: 2.0.0
FROM registry.baidubce.com/paddlepaddle/paddle:2.6.1-gpu-cuda12.0-cudnn8.9-trt8.6

# PaddleOCR base on Python3.7
RUN pip3.10 install --upgrade pip -i https://pypi.tuna.tsinghua.edu.cn/simple

RUN pip3.10 install paddlehub --upgrade -i https://pypi.tuna.tsinghua.edu.cn/simple

# If you want to use PP-OCRv4. You need to modify the `model_dir` fields in the file of `deploy/hubserving/ocr_system/params.py`
RUN git clone https://github.com/caoyuan-patsnap/PaddleOCR.git /PaddleOCR

WORKDIR /PaddleOCR

RUN pip3.10 install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

RUN pip3.10 install protobuf==3.20.0 -i https://pypi.tuna.tsinghua.edu.cn/simple

RUN mkdir -p /PaddleOCR/inference/

# Tip: You can download models from https://github.com/PaddlePaddle/PaddleOCR/blob/release/2.7/doc/doc_ch/models_list.md
ADD https://paddleocr.bj.bcebos.com/PP-OCRv4/chinese/ch_PP-OCRv4_det_infer.tar /PaddleOCR/inference/
ADD https://paddleocr.bj.bcebos.com/PP-OCRv4/chinese/ch_PP-OCRv4_rec_infer.tar /PaddleOCR/inference/
ADD https://paddleocr.bj.bcebos.com/PP-OCRv3/multilingual/korean_PP-OCRv3_rec_infer.tar /PaddleOCR/inference/
ADD https://paddleocr.bj.bcebos.com/PP-OCRv3/multilingual/japan_PP-OCRv3_rec_infer.tar /PaddleOCR/inference/
ADD https://paddleocr.bj.bcebos.com/dygraph_v2.0/ch/ch_ppocr_mobile_v2.0_cls_infer.tar /PaddleOCR/inference/

ADD https://paddleocr.bj.bcebos.com/ppstructure/models/layout/picodet_lcnet_x1_0_fgd_layout_infer.tar /PaddleOCR/inference/
ADD https://paddleocr.bj.bcebos.com/ppstructure/models/slanet/ch_ppstructure_mobile_v2.0_SLANet_infer.tar /PaddleOCR/inference/
ADD https://paddleocr.bj.bcebos.com/ppstructure/models/vi_layoutxlm/ser_vi_layoutxlm_xfund_infer.tar /PaddleOCR/inference/
ADD https://paddleocr.bj.bcebos.com/ppstructure/models/vi_layoutxlm/re_vi_layoutxlm_xfund_infer.tar /PaddleOCR/inference/

# Download orc detect model(light version). if you want to change normal version, you can change ch_ppocr_mobile_v2.0_det_infer to ch_ppocr_server_v2.0_det_infer, also remember change det_model_dir in deploy/hubserving/ocr_system/params.py）
RUN tar xf /PaddleOCR/inference/ch_PP-OCRv4_det_infer.tar -C /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/ch_PP-OCRv4_rec_infer.tar -C /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/korean_PP-OCRv3_rec_infer.tar -C /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/japan_PP-OCRv3_rec_infer.tar -C /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/ch_ppocr_mobile_v2.0_cls_infer.tar -C /PaddleOCR/inference/

RUN tar xf /PaddleOCR/inference/picodet_lcnet_x1_0_fgd_layout_infer.tar -C /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/ch_ppstructure_mobile_v2.0_SLANet_infer.tar -C /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/ser_vi_layoutxlm_xfund_infer.tar -C /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/re_vi_layoutxlm_xfund_infer.tar -C /PaddleOCR/inference/

EXPOSE 8868

CMD ["/bin/bash","-c","hub install deploy/hubserving/ocr_det/ && hub install deploy/hubserving/ocr_cls && hub install deploy/hubserving/ocr_rec && hub install deploy/hubserving/ocr_system/ && hub install deploy/hubserving/structure_table/ && hub install deploy/hubserving/structure_system/ && hub install deploy/hubserving/structure_system/ && hub install deploy/hubserving/structure_layout/ && hub install deploy/hubserving/kie_ser/ && hub install deploy/hubserving/kie_ser_re/ && hub serving start -m ocr_system"]
