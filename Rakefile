def get_homedir
  require 'yaml'
  configdata = YAML.load_file "./_config.yaml"
  return configdata['home']
end

def get_moduleTitle(moduleFolder)
  require 'yaml'
  fileToLoad = moduleFolder + '/_config.yaml'
  return if File.file?(fileToLoad) == false
  configdata = YAML.load_file fileToLoad
  return configdata['title']
end

task :default => ["help"]

desc "Print command to obtain task descriptions"
task :help do
  puts "------------------------------------------------"
  puts "Use rake -T to obtain a list of available"
  puts "and a description of what they do."
  puts "------------------------------------------------"
  puts "readme -- creates Readme files for modules and images"
end

def write_image_readme(imageFolder,footerLine)
	i = 0
	return if File.file?(imageFolder + '/_config.yaml') == false
	imageData = YAML.load_file "#{imageFolder}/_config.yaml"
    keys = imageData.keys
    figs = keys.sort
    while i < keys.count
      titleLine = '# ' + imageData[figs[i]]['title'] + "\n\n"
      linkLine = '[' + imageData[figs[i]]['source'] + '](' + imageData[figs[i]]['filename'] + '.' + imageData[figs[i]]['extension'] + ")\n\n"
      captionLine = imageData[figs[i]]['caption'] + "\n\n"
      imageLine = '![' + imageData[figs[i]]['filename'] + '](' + imageData[figs[i]]['filename'] + '.png' + ')' + "\n\n"
      fnameReadme = imageFolder + '/Readme.md'
      somefile = File.open(fnameReadme, "w")
      somefile.print titleLine
      somefile.print linkLine
      somefile.print captionLine
      somefile.print imageLine
      somefile.print footerLine
      somefile.close
      i +=1
    end
end

def add_to_module_readme(imageFolder,footerLine,mrme)
  i = 0
  return if File.file?(imageFolder + '/_config.yaml') == false
  imageData = YAML.load_file "#{imageFolder}/_config.yaml"
    keys = imageData.keys
    figs = keys.sort
    while i < keys.count
      titleLine = '## ' + imageData[figs[i]]['title'] + "\n"
      linkLine = '[More Detail](' + imageData[figs[i]]['filename'] + '/Readme.md)' + ' | ' '[' + imageData[figs[i]]['source'] + '](' + imageData[figs[i]]['filename'] + '/' + imageData[figs[i]]['filename'] +  '.' + imageData[figs[i]]['extension'] + ")\n\n"
      imageLine = '![' + imageData[figs[i]]['filename'] + '](' + imageData[figs[i]]['filename'] + '/' + imageData[figs[i]]['filename'] + '.png)' + "\n\n"
      i +=1
    end
  mrme = mrme + titleLine + linkLine + imageLine
  return mrme
end

def write_module_readme(moduleFolder,mrme)
  fnameReadme = moduleFolder + '/Readme.md'
  somefile = File.open(fnameReadme, "w")
  somefile.print mrme
  somefile.close
end

task :readme do
  require 'yaml'
  homedir = get_homedir
  footer = File.read(homedir + '/footer.md')
  moduleFolders = Dir.glob(homedir + '/*').reject {|e| !File.directory?(e)}
  moduleFolders.each do |mf|
    moduleTitle = get_moduleTitle(mf)
    moduleReadme = '# ' + moduleTitle + "\n\n"
  	imageFolders = Dir.glob(mf + '/*').reject {|e| !File.directory?(e)}
  	imageFolders. each do |imf|
  		write_image_readme(imf,footer)
      moduleReadme = add_to_module_readme(imf,footer,moduleReadme)
  	end
    moduleReadme = moduleReadme + footer
    write_module_readme(mf,moduleReadme)
  end
end