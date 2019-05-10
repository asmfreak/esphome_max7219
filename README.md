# MAX7219 Grid display
## Installation
1. `git clone https://github.com/ASMfreaK/esphome_max7219.git max7219grid`
2. Use this example configuration

```yaml

esphome:
  # ...
  includes:
    - max7219grid/max7219grid.hpp

# ...
spi:
  clk_pin: D1
  mosi_pin: D2
  id: _spi

custom_component:
- lambda: |-
    using Disp = esphome::display::MAX7219GridComponent;
    auto grid = new Disp(id(_spi), new esphome::GPIOPin(D3, OUTPUT), 100);
    grid->set_num_chips(4);
    auto writer = [](Disp& it){
      static uint16_t offset=0;
      ++offset;
      for(uint16_t l=0; l< it.width(); ++l){
        it.fill_vertical_line(l, 0x1 << ((l + offset) % it.height()));
      }
    };
    grid->set_writer(writer);
    return {grid};

```
