**Análise e Processamento de Imagens com o Pipeline JWST Stage 1**

Nesta análise, utilizamos Python juntamente com a biblioteca `jwst`, que é uma biblioteca para calibração de observações científicas do Telescópio Espacial James Webb. Você pode instalá-la usando:
\```
pip install jwst
\```

O Telescópio Espacial James Webb (JWST) é uma das missões astronômicas mais esperadas. O JWST tem um complexo pipeline de processamento de imagens, e neste documento, vamos nos aprofundar na Etapa 1 desse pipeline.

**Etapas Técnicas do Pipeline Stage 1**

1. **Group Scale Correction**
Esta etapa normaliza a exposição corrigindo variações de fluxo entre grupos consecutivos.
\```
from jwst.group_scale import GroupScaleStep
scaled = GroupScaleStep.call(uncalibrated_data)
\```

2. **Saturation Flagging**
Nesta etapa, os pixels que excedem um determinado limite de saturação são identificados e marcados.
\```
from jwst.saturation import SaturationStep
saturated = SaturationStep.call(scaled)
\```

3. **IPC Correction**
Esta etapa aplica a correção de interferência entre pixels.
\```
from jwst.ipc import IPCStep
ipc_corrected = IPCStep.call(saturated)
\```

4. **Superbias Subtraction**
Subtrai uma imagem de bias médio de muitas leituras não iluminadas.
\```
from jwst.superbias import SuperBiasStep
superbias = SuperBiasStep.call(ipc_corrected)
\```

5. **RefPix Correction**
Esta etapa utiliza os pixels de referência para corrigir o ruído eletrônico.
\```
from jwst.refpix import RefPixStep
refpix_corrected = RefPixStep.call(superbias)
\```

6. **Jump Detection**
Identifica desvios súbitos entre grupos consecutivos.
\```
from jwst.jump import JumpStep
jumps_detected = JumpStep.call(refpix_corrected)
\```

7. **Ramp Fitting**
Ajusta uma reta à sequência de leituras em um pixel para produzir uma imagem de taxa de contagem.
\```
from jwst.ramp_fitting import RampFitStep
slope, slope_int = RampFitStep.call(jumps_detected)
\```

**Funções Auxiliares**

A função `show_image` é usada para visualizar as imagens em várias etapas.
\```
import matplotlib.pyplot as plt

def show_image(data, vmin, vmax):
    plt.figure(figsize=(12,12))
    plt.imshow(data, origin='lower', cmap='gray', vmin=vmin, vmax=vmax)
    plt.colorbar().set_label('DN/s')
    plt.show()
\```
