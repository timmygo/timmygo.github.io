# Introduction to Tuning work-flow

---

The tuning work flow would be the following:

- Black level calibration
- AWB/CCM calibration
- LSH calibration
- LCA calibration
- Primary Denoiser tuning
- Sharpening tuning/Secondary denoiser
- Tonemapping tuning

## Before we started

- Make sure the surface of sensors and lens clean and clear.
- Get the prime lens focus by manual.
- Turn off 3A fucntion, set gain to 1x and take pictures with appropriate expourse time which means not to be overexposure or underexposure.

## Black level calibration

- Get the sensor under darkness, then take pictures in rawdata format.
- Statistics about individual values of four components in the bayer RGGB (red, green1, green2, blue) indicate the offset value which should be cut off to reduce the dark current effect.

## AWB/CCM calibration

- We would like to place the color checker in a color light box, and take pictures under four color temperatures (D65, D50, CWF and A Light).
- Tunning tools then utilize the raw picture data, and output a group of parameters which are color correction martixs regard to four color temperatures.
- ISP should be able to calculate out the correct gain value of red, green1, green2 and blue for color constancy according to the parameters.

## LSH calibration

- The camera would be illuminated with a uniform light source by a light panel. Then pictures get captured.
- Tunning tools generate the deshading matrix with the rawdata.

## LCA calibration

-  The camera would be illuminated by the TE-251 chart with D65 light source. Then pictures get captured.
-  Like LSH calibration, tunning tools generate parameters with the rawdata.

## Primary Denoiser tuning
   > Adjusting parameters below in tunning tools.

- DNS_STRENGTH 
  > Denoising strength.

- DNS_GREYSC_PIXTHRESH_MULT 
  > Pixel threshold multiplier used to adjust the sensitivity of colour differences for the greyscale weights. 

## Sharpening tuning/Secondary denoiser

1. The scene for tuning sharpness would typically be a lab scene with charts and real objects. A chart like ISO12233 is a good choice and real objects like dolls, wool (items that have fine texture).
2. Change the **sharpening radius** in 0.5 increments until it gets to 2.5 and determine optimal value.
3. Change **sharpening strength** in 0.1 increment until reasonable results at edges are achieved.
4. Change **sharpening details** in 0.1 decrement (if necessary) to reduce noise in flat regions of the image without removing the sharpening at edges. 
5. After tuning these values it should be validated over a number of scenes/light levels to understand if further fine tuning is required.
6. The **secondary denoiser** has got two controls and assuming that sharpening has been done before the process would be as below:
- Increase the **secondary denoiser strengh** to increase noise reduction.
- Increase the **edge avoidance** to perform more edge avoidance.

## Tonemapping tuning

- **Tone Mapping** is to map one set of colors to another to approximate the appearance of high-dynamic-range images in a medium that has a more limited dynamic range. 

- The **automatic tone mapping control**  (TNMC)is in charge of:
  - Dynamically generate a global mapping curve based on previous captures’ statistics.
  - Control the strength of the tone mapped module so tone mapping strength is reduced when the sensor is setup in a way which may produce noisy images (basically when Sensor’s gain value is high).

- We would like to modify the default gamma curve in tunning tools to generate a new curve (TNMC works based on it) for desired brightness, contrast and saturation.

[back](./)
