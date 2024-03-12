# PWM LED breathing light implementation

## introduce
Breathing light module implemented using Verilog. This module simulates the effect of a breathing light by changing the brightness of the LED. The implementation principle is to use a counter to control the brightness change speed of the LED.

##Verilog code

```verilog
module pwm_LED(
    input         clk, 
    input         rst_n,
    output reg    led
);

reg [18:0] cnt0;
wire add_cnt0 = 1;
wire end_cnt0 = add_cnt0 && (cnt0 == 400_000 - 1);

reg [7:0] cnt1;
wire add_cnt1 = end_cnt0;
wire end_cnt1 = add_cnt1 && (cnt1 == 125 - 1);

reg [3:0] cnt2;
wire add_cnt2 = end_cnt1;
wire end_cnt2 = add_cnt2 && (cnt2 == 6 - 1);

reg [18:0] led_1to0;

always @(posedge clk or negedge rst_n) begin
    if (!rst_n) begin
        cnt0 <= 0;
    end else if (add_cnt0) begin
        if (end_cnt0) cnt0 <= 0;
        else cnt0 <= cnt0 + 1;
    end
end

always @(posedge clk or negedge rst_n) begin
    if (!rst_n) begin
        cnt1 <= 0;
    end else if (add_cnt1) begin
        if (end_cnt1) cnt1 <= 0;
        else cnt1 <= cnt1 + 1;
    end
end

always @(posedge clk or negedge rst_n) begin
    if (!rst_n) begin
        cnt2 <= 0;
    end else if (add_cnt2) begin
        if (end_cnt2) cnt2 <= 0;
        else cnt2 <= cnt2 + 1;
    end
end

always @(posedge clk or negedge rst_n) begin
    if (!rst_n) begin
        led <= 1;
    end else if (add_cnt0 && cnt0 == led_1to0 - 1) begin
        led <= 0;
    end else if (end_cnt0) begin
        led <= 1;
    end
end

always @(*) begin
    case (cnt2)
        0: led_1to0 = 380_000;
        1: led_1to0 = 340_000;
        2: led_1to0 = 280_000;
        3: led_1to0 = 200_000;
        4: led_1to0 = 80_000;
        default: led_1to0 = 10_000;
    endcase
end

endmodule

## Pin assignment
clk	 Input	PIN_AF14
led	 Output	PIN_AB23
rst_n	Input	PIN_AJ4




