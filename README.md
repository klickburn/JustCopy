def custom_formatter(x, pos):
    return '{}K'.format(int(x//1000))

ax.yaxis.set_major_formatter(FuncFormatter(custom_formatter))
