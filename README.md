# BYOL-idea-implement

[Bootstrap your own latent: A new approach to self-supervised Learning](https://arxiv.org/abs/2006.07733)에서 제시한 내용에 대한 아이디어를 다음 순서로 진행되는 실험을 통해 얻었다고 한다.

1.  A 라는 network를 random initialization 시킨 뒤 weight를 고정시킵니다. 여기에 Self-Supervised Learning의 evaluation protocol인 linear evaluation protocol을 통해 ImageNet 데이터셋에 대한 정확도를 측정합니다. 즉, random init 후 freeze 시킨 feature extractor에 1개의 linear layer를 붙여서 ImageNet 데이터셋로 학습시킨 뒤 정확도를 측정하는 것입니다. 이 경우, feature extractor가 아무런 정보도 배우지 않은 채 linear layer만 붙였기 때문에 좋은 성능을 얻을 수 없고, 실제로 1.4%의 top-1 accuracy를 달성하였다고 합니다.

2.  unlabeled dataset을 random initialized A network + MLP (뒤에서 자세히 설명드릴 예정)에 feed forward 시켜서 prediction들을 얻어냅니다.

3.  B 라는 network를 하나 준비합니다. B도 마찬가지로 random initialization 시키는데, 바로 linear evaluation을 수행하지 않고, image들을 A network에 feed forward 시켜서 뽑아낸 prediction을 target으로 하여 이 target을 배우도록 학습을 시킵니다. 그 결과 1.4점의 시험 결과를 얻습니다.

놀랍게도, B network는 random initialized A network가 내뱉은 부정확한 prediction들을 배우도록 학습을 한 뒤 linear evaluation을 하였을 때 18.8%의 top-1 accuracy를 얻을 수 있었다고 합니다. 물론 이 수치는 매우 낮은 수치입니다. 하지만 random initialization을 하였을 때 보다는 굉장히 큰 폭으로 성능이 증가한 셈이죠. 이 간단한 실험이 BYOL의 core motivation이 되었다고 저자들은 서술하고 있습니다.

출처: https://hoya012.github.io/blog/byol

위 내용을 바탕으로 진짜인지 확인해보기 위해 코드를 직접 작성하여 실험을 진행해보았다.

---

실험 결과 실제로 성능이 증가함을 확인하였다. 현재 8%의 성능을 보이고 있으며, Epochs를 늘리거나 Image Transforms 방법을 더 복잡하게 할 경우 성능이 더 증가할 것을 확인 할 수 있었다.
