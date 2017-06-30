# Active File

...

## Example

class Person < ApplicationRecord
  has_one :avatar
end

class Avatar < ApplicationRecord
  belongs_to :person
  belongs_to :image, class_name: 'ActiveFile::Blob'

  has_file :image
end

avatar.image.url(expires_in: 5.minutes)


class ActiveFile::DownloadsController < ActionController::Base
  def show
    head :ok, ActiveFile::Blob.locate(params[:id]).download_headers
  end
end


class AvatarsController < ApplicationController
  def create
    # @avatar = Avatar.create \
    #   image: ActiveFile::Blob.save!(file_name: params.require(:name), content_type: request.content_type, data: request.body)
    @avatar = Avatar.create! image: Avatar.image.extract_from(request)
  end
end


class ProfilesController < ApplicationController
  def update
    @person.update! avatar: @person.avatar.update!(image: )
  end
end

## License

Google Sign-In for Rails is released under the [MIT License](https://opensource.org/licenses/MIT).