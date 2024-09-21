import discord, os, random
import requests
from discord.ext import commands


intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='!', intents=intents)

random_facts = ['Стекло разлагается 2000 лет.', 'Пластик разлагается 1000 лет.', 'Создание и утилизация литьевых батарей сильно вредит экологии.']


@bot.event
async def on_ready():
    print(f'We have logged in as {bot.user}')

@bot.command()
async def hello(ctx):
    await ctx.send(f'Привет! Я бот {bot.user}!')

@bot.command()
async def bye(ctx):
    await ctx.send(f'Пока-пока!')

@bot.command()
async def heh(ctx, count_heh = 5):
    await ctx.send("хе" * count_heh)

@bot.command()
async def add(ctx, left: int, right: int):
    """Adds two numbers together."""
    await ctx.send(left + right)

@bot.command()
async def joined(ctx, member: discord.Member):
    """Says when a member joined."""
    await ctx.send(f'{member.name} вступил {discord.utils.format_dt(member.joined_at)}')

@bot.group()
async def cool(ctx):
    """Says if a user is cool.

    In reality this just checks if a subcommand is being invoked.
    """
    if ctx.invoked_subcommand is None:
        await ctx.send(f'Нет, {ctx.subcommand_passed} не крутой')


@cool.command(name='бот')
async def _bot(ctx):
    """Is the bot cool?"""
    await ctx.send('Да, он крутой')

@bot.command()
async def repeat(ctx, times: int, content='повторяю...'):
    """Repeats a message multiple times."""
    for i in range(times):
        await ctx.send(content)


@bot.command()
async def whatsup(ctx):
    await ctx.send(f'Все супер.')

def get_duck_image_url():    
    url = 'https://random-d.uk/api/random'
    res = requests.get(url)
    data = res.json()
    return data['url']

def get_dog_image_url():   
    url = 'https://random.dog/woof.json' 
    res = requests.get(url)
    data = res.json()
    return data['url']


def get_fox_image_url():   
    url = 'https://randomfox.ca/floof/' 
    res = requests.get(url)
    data = res.json()
    return data['url']


@bot.command('duck')
async def duck(ctx):
    '''По команде duck вызывает функцию get_duck_image_url'''
    image_url = get_duck_image_url()
    await ctx.send(image_url)

@bot.command('dog')
async def duck(ctx):
    '''По команде duck вызывает функцию get_duck_image_url'''
    image_url = get_dog_image_url()
    await ctx.send(image_url)

@bot.command('fox')
async def duck(ctx):
    '''По команде duck вызывает функцию get_duck_image_url'''
    image_url = get_fox_image_url()
    await ctx.send(image_url)

@bot.command()
async def fact(ctx):
    random_element = random.choice(random_facts)
    await ctx.send(f'Случайный факт: {random_element}')






bot.run('')


