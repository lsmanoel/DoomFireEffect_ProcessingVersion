# DoomFireEffect ProcessingVersion
http://fabiensanglard.net/doom_fire_psx/

![](https://raw.githubusercontent.com/lsmanoel/DoomFireEffect_ProcessingVersion/master/Screenshot%20from%202019-02-02%2001-59-32.png)
```Processing
public class Fire {
  int window_size_x; int window_size_y;
  int[][] pixel_map; 
  int fire_length; int fire_power; int fire_decay;
  int pixel_size=20;
  int[] R = {0x07, 0x1f, 0x2f, 0x47, 0x57, 0x67, 0x77, 0x8f, 0x9f, 0xaf, 0xb7, 0xc7, 0xdf, 0xdf, 0xdf, 0xd7, 0xd7, 0xc7, 0xcf, 0xcf, 0xcf, 0xc7, 0xc7, 0xc7, 0xbf, 0xbf, 0xbf, 0xbf, 0xbf, 0xb7, 0xb7, 0xb7, 0xcf, 0xdf, 0xef, 0xff};
  int[] G = {0x07, 0x07, 0x0f, 0x0f, 0x17, 0x17, 0x1f, 0x27, 0x2f, 0x3f, 0x47, 0x47, 0x4f, 0x57, 0x57, 0x5f, 0x67, 0x6f, 0x77, 0x7f, 0x87, 0x87, 0x8f, 0x97, 0x9f, 0x9f, 0xa7, 0xa7, 0xaf, 0xaf, 0xb7, 0xb7, 0xcf, 0xdf, 0xef, 0xff};
  int[] B = {0x07, 0x07, 0x07, 0x07, 0x07, 0x07, 0x07, 0x07, 0x07, 0x07, 0x07, 0x07, 0x07, 0x07, 0x07, 0x07, 0x0f, 0x0f, 0x0f, 0x0f, 0x17, 0x17, 0x17, 0x1f, 0x1f, 0x1f, 0x27, 0x27, 0x2f, 0x2f, 0x2f, 0x37, 0x6f, 0x9f, 0xc7, 0xff};
  int random_table_size = 5;
  int[] random_table = {0, 0, 0, 1, 1};  
  public Fire() {}
  public Fire(int fire_power_input, int window_size_x_input, int window_size_y_input) {
    window_size_x = window_size_x_input; 
    window_size_y = window_size_y_input;
    fire_decay = fire_power_input; fire_power = fire_power_input;   
    pixel_map= new int[window_size_x+2*fire_power][window_size_y+2*fire_power];
  }
  public Fire(int fire_power_input, int window_size_x_input, int window_size_y_input, int fire_decay_input) {
    window_size_x = window_size_x_input; 
    window_size_y = window_size_y_input;
    fire_decay = fire_decay_input; fire_power = fire_power_input;   
    pixel_map= new int[window_size_x+2*fire_power][window_size_y+2*fire_power];
  }
  public Fire(int fire_power_input, int window_size_x_input, int window_size_y_input, int fire_decay_input,int fire_length_input) {
    window_size_x = window_size_x_input; 
    window_size_y = window_size_y_input;
    fire_decay = fire_decay_input; fire_power = fire_power_input; fire_length = fire_length_input;
    pixel_map= new int[window_size_x+2*fire_power][window_size_y];    
  }
  
  void floor_place_fire_source(int higher){
    for(int i=window_size_y-higher; i<window_size_y; i++){for(int j=fire_power; j<window_size_x; j++){pixel_map[j][i]=35;}}  
  }
  
  void place_circle_fire_source(int radius, int cx, int cy){
    if(cy>radius && cy < window_size_y-fire_power-radius && cx>radius && cx < window_size_x-fire_power-radius){
      for(int i=cy-radius; i<cy+radius; i++){
        for(int j=cx-radius; j<cx+radius; j++){
          if(((i-cy)*(i-cy)+(j-cx)*(j-cx))<(radius*radius))
          pixel_map[j+fire_power][i+fire_power]=35;
        }
      } 
    }
  }
  
  int decay_random(int fire_decay){
     int r = int(random(fire_decay));
     if (r>(2)){
       r=0;
     }
     //println(r);
     return r;
  }
  void fire_update(){
    for(int i=fire_power; i<window_size_y; i++){
      for(int j=0; j<window_size_x; j++){
        if(pixel_map[j+fire_power][i]<35 && pixel_map[j+fire_power][i]>27+random(5)){
          pixel_map[j+fire_power-int(random(fire_power))+int(random(fire_power))][i+int(random(fire_power))]=pixel_map[j+fire_power][i]-int(random(fire_decay))-2;
        }
        if(pixel_map[j+fire_power][i]>4){
          pixel_map[j+fire_power-int(random(fire_power))+int(random(fire_power))][i-int(random(fire_power))]=pixel_map[j+fire_power][i]-random_table[int(random(random_table_size))];
        }
        if(pixel_map[j+fire_power][i]>0){
          pixel_map[j+fire_power][i]=pixel_map[j+fire_power][i]-random_table[int(random(random_table_size))];     
        }
      }
    }
  }
  
  void screen_update(){
    loadPixels();
    for(int i=fire_power; i<window_size_y; i++){
      for(int j=0; j<window_size_x; j++){
        if(pixel_map[j+fire_power][i]>0){pixels[(i*window_size_x)+j] = color(R[pixel_map[j+fire_power][i]], G[pixel_map[j+fire_power][i]], B[pixel_map[j+fire_power][i]]);}
      }
    }
    updatePixels();
  }
  
  void palette_color_show() {for (int i=0; i<36; i++) {fill(R[i], G[i], B[i]); square(pixel_size*i, 0, pixel_size);}}
}
```
